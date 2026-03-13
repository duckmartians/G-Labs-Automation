# G-Labs Webhook API — Integration Guide

> **Base URL:** `http://127.0.0.1:8765`  
> **Auth:** API Key via `X-API-Key` header  
> **License:** MAX plan required

---

## 🔑 Authentication

All endpoints (except `/api/health`) require an API key in the request header:

```
X-API-Key: YOUR_API_KEY
```

The API key is auto-generated in **Settings → Webhook** tab. You can regenerate it anytime.

**401 Unauthorized response:**
```json
{
  "error": "Unauthorized — invalid or missing API key"
}
```

---

## 📡 Endpoints

### 1. Health Check

```
GET /api/health
```

No authentication required. Use this to verify the server is running.

**Response `200`:**
```json
{
  "status": "ok",
  "server": "G-Labs Webhook",
  "uptime": 3600,
  "tasks_pending": 0,
  "tasks_running": 1
}
```

---

### 2. Generate Image

```
POST /api/image/generate
Content-Type: application/json
X-API-Key: YOUR_API_KEY
```

**Request body:**

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `prompt` | string | ✅ | — | Image description |
| `model` | string | — | `imagen4` | `imagen4`, `nano_banana`, `nano_banana_2`, `nano_banana_pro` |
| `count` | integer | — | `1` | Number of images (1–8) |
| `aspect_ratio` | string | — | `1:1` | `1:1`, `16:9`, `9:16`, `4:3`, `3:4` |
| `reference_images` | array | — | `[]` | List of base64 images or objects (see below) |
| `upscale` | array | — | `[]` | List of formats: `["2K"]`, `["4K"]`, or `["2K", "4K"]` |

**Example:**
```bash
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "a cat wearing sunglasses on the beach",
    "model": "imagen4",
    "count": 2,
    "aspect_ratio": "1:1"
  }'
```

**Reference Image Format:**
```json
"reference_images": [
  "data:image/png;base64,...", 
  {"data": "base64...", "category": "subject"}
]
```
Categories (Whisk only): `subject`, `scene`, `style`.

**Upscale Format:**
```json
"upscale": ["2K", "4K"]
```

**Response `202`:**
```json
{
  "task_id": "a1b2c3d4",
  "status": "pending",
  "message": "Task queued for processing",
  "poll_url": "/api/status/a1b2c3d4"
}
```

---

### 3. Generate Video

```
POST /api/video/generate
Content-Type: application/json
X-API-Key: YOUR_API_KEY
```

**Request body:**

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `prompt` | string | ✅ | — | Video description |
| `model` | string | — | `veo_31_fast_relaxed` | `veo_31_fast_relaxed`, `veo_31_fast`, `veo_31_quality` |
| `mode` | string | — | `text_to_video` | `text_to_video`, `start_image`, `start_end_image`, `components` |
| `aspect_ratio` | string | — | `16:9` | `16:9`, `9:16` |
| `resolution` | array | — | `["720p"]` | List: `["720p"]`, `["1080p"]`, `["4K"]` |
| `reference_images` | array | — | `[]` | Base64 images. Required for non-text modes. |

**Available video models:**

| `model` value | Display Name | Credits | Note |
|---|---|---|---|
| `veo_31_fast_relaxed` | Veo 3.1 Fast Relaxed | 0 | Default. Low priority (ULTRA only) |
| `veo_31_fast` | Veo 3.1 Fast | 10 | Normal priority |
| `veo_31_quality` | Veo 3.1 Quality | 100 | Highest quality |

**Example:**
```bash
curl -X POST http://127.0.0.1:8765/api/video/generate \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "a golden retriever running on the beach, slow motion",
    "model": "veo_31_fast",
    "aspect_ratio": "16:9"
  }'
```

**Response `202`:** Same format as image generate.

---

### 4. Check Task Status

```
GET /api/status/{task_id}
X-API-Key: YOUR_API_KEY
```

**Response `200` (pending/running):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "running",
  "prompt": "a cat wearing sunglasses...",
  "created_at": 1709500000
}
```

**Response `200` (completed):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "completed",
  "prompt": "a cat wearing sunglasses...",
  "created_at": 1709500000,
  "completed_at": 1709500120,
  "results": [
    "http://127.0.0.1:8765/api/files/image_001.jpg",
    "http://127.0.0.1:8765/api/files/image_002.jpg"
  ]
}
```

**Response `200` (failed):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "failed",
  "error_code": 403,
  "error": "FORBIDDEN",
  "error_detail": "Account blocked"
}
```

**Status values:** `pending` → `running` → `completed` / `failed`

---

### 5. Get Task Result

```
GET /api/result/{task_id}
X-API-Key: YOUR_API_KEY
```

Returns only the result data (download URLs). Similar to status but focused on output files.

---

### 6. List All Tasks

```
GET /api/tasks
X-API-Key: YOUR_API_KEY
```

**Response `200`:**
```json
{
  "tasks": [
    {
      "task_id": "a1b2c3d4",
      "type": "image",
      "status": "completed",
      "prompt": "a cat wearing sunglasses...",
      "created_at": 1709500000
    }
  ]
}
```

Returns up to 50 most recent tasks, sorted newest first.

---

### 7. Download File

```
GET /api/files/{filename}
X-API-Key: YOUR_API_KEY
```

Returns the binary file (image/video) with appropriate `Content-Type` header.

---

## 🔄 Polling Pattern

Since generation takes time (10s–5min), use polling to check task completion:

```python
import requests
import time

API_KEY = "YOUR_API_KEY"
BASE = "http://127.0.0.1:8765"
HEADERS = {"X-API-Key": API_KEY, "Content-Type": "application/json"}

# 1. Submit task
resp = requests.post(f"{BASE}/api/image/generate", headers=HEADERS, json={
    "prompt": "a futuristic city at sunset",
    "model": "imagen4",
    "count": 2,
    "aspect_ratio": "16:9"
})
task_id = resp.json()["task_id"]
print(f"Task submitted: {task_id}")

# 2. Poll until done
while True:
    status = requests.get(f"{BASE}/api/status/{task_id}", headers=HEADERS).json()
    print(f"Status: {status['status']}")
    
    if status["status"] == "completed":
        for url in status["results"]:
            # 3. Download files
            img = requests.get(url, headers=HEADERS)
            filename = url.split("/")[-1]
            with open(filename, "wb") as f:
                f.write(img.content)
            print(f"Saved: {filename}")
        break
    elif status["status"] == "failed":
        print(f"Error: {status['error']}")
        break
    
    time.sleep(5)  # Wait 5s between polls
```

---

## 🔗 Integration Examples

### n8n Workflow

1. **HTTP Request node** → POST `http://127.0.0.1:8765/api/image/generate`
2. Set header: `X-API-Key: YOUR_KEY`
3. **Wait node** → 30 seconds
4. **HTTP Request node** → GET `http://127.0.0.1:8765/api/status/{{task_id}}`
5. **IF node** → Check `status == "completed"`
6. **HTTP Request node** → GET result file URLs

### JavaScript / Node.js

```javascript
const API_KEY = "YOUR_API_KEY";
const BASE = "http://127.0.0.1:8765";

// Submit
const res = await fetch(`${BASE}/api/image/generate`, {
  method: "POST",
  headers: { "X-API-Key": API_KEY, "Content-Type": "application/json" },
  body: JSON.stringify({
    prompt: "a cat in space",
    model: "imagen4",
    aspect_ratio: "1:1"
  })
});
const { task_id } = await res.json();

// Poll
const poll = setInterval(async () => {
  const status = await fetch(`${BASE}/api/status/${task_id}`, {
    headers: { "X-API-Key": API_KEY }
  }).then(r => r.json());
  
  if (status.status === "completed") {
    clearInterval(poll);
    console.log("Files:", status.results);
  }
}, 5000);
```

### cURL Quick Test

```bash
# Health check
curl http://127.0.0.1:8765/api/health

# Generate image
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "X-API-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "hello world", "model": "imagen4"}'

# Check status
curl -H "X-API-Key: YOUR_KEY" http://127.0.0.1:8765/api/status/TASK_ID

# List tasks
curl -H "X-API-Key: YOUR_KEY" http://127.0.0.1:8765/api/tasks
```

---

## ⚠️ Error Codes

| HTTP Code | Meaning |
|---|---|
| `200` | Success |
| `202` | Task accepted (queued) |
| `400` | Bad request (missing prompt, invalid JSON) |
| `401` | Invalid or missing API key |
| `404` | Task/file/endpoint not found |
| `500` | Internal server error |
