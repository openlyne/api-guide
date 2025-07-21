# OTP Send and Verify API

This API lets you send and verify One-Time Passwords (OTPs) for user authentication. It's easy to use with your API key!

## Endpoints

### 1. Send OTP

- **URL**: `/send-otp`
- **Method**: `POST`
- **Headers**:
  - `x-api-key`: Your API key

**Body**:
- `phone`: Phone number with country code (e.g., `256700123456`)
- `uid`: User ID (a UUID like `123e4567-e89b-12d3-a456-426614174000`)
- `env`: (optional) Use `"sandbox"` for testing; leave it out for live mode

**What it does**: Sends a 6-digit OTP to the phone and saves it with the uid.

#### Example Request:
```bash
curl -X POST https://openlyne.sliplane.app/webhook/send-otp \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{"phone": 256700123456, "uid": "123e4567-e89b-12d3-a456-426614174000", "env": "sandbox"}'
```

#### Responses:

- **200 OK**: OTP sent!
  ```json
  {
    "success": true,
    "data": {
      "status": "Sent",
      "number": "256700123456"
    },
    "message": "OTP sent successfully",
    "error": null
  }
  ```

- **400 Bad Request**: Check your phone or uid
- **401 Unauthorized**: Wrong API key
- **429 Too Many Requests**: Wait 30 seconds before trying again
- **500 Internal Server Error**: Something went wrong on our end

### 2. Verify OTP

- **URL**: `/verify-otp`
- **Method**: `POST`
- **Headers**:
  - `x-api-key`: Your API key

**Body**:
- `uid`: User ID (same UUID as before)
- `code`: The 6-digit OTP you received

**What it does**: Checks if the OTP matches and is within 3 minutes. If valid, it's deleted.

#### Example Request:
```bash
curl -X POST https://openlyne.sliplane.app/webhook/verify-otp \
  -H "x-api-key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{"uid": "123e4567-e89b-12d3-a456-426614174000", "code": "123456"}'
```

#### Responses:

- **200 OK**: OTP verified!
  ```json
  {
    "success": true,
    "data": {
      "message": "OTP verified successfully",
      "contact_value": "256700123456"
    },
    "message": "Verification successful",
    "error": {}
  }
  ```

- **400 Bad Request**: OTP is wrong or expired
- **401 Unauthorized**: Wrong API key
- **500 Internal Server Error**: Something went wrong on our end

## Quick Tips

- Always include your API key in the `x-api-key` header
- `phone` needs a country code (e.g., 256 for Uganda)
- `uid` must be a valid UUID
- OTPs expire after 3 minutes
- You can only request a new OTP every 30 seconds for the same uid
- Use `"env": "sandbox"` in the Send OTP body for testing, or skip it for real SMS

## Code Examples

Below are examples of how to use the API in Python and JavaScript. Replace `YOUR_API_KEY` with your actual API key.

<details>
<summary>🐍 Python Example</summary>

```python
import requests
import json

# Base URL for your n8n instance
BASE_URL = "https://openlyne.sliplane.app/webhook"
API_KEY = "YOUR_API_KEY"
HEADERS = {"x-api-key": API_KEY, "Content-Type": "application/json"}

def send_otp(phone, uid, env="sandbox"):
    url = f"{BASE_URL}/send-otp"
    data = {"phone": phone, "uid": uid, "env": env}
    try:
        response = requests.post(url, headers=HEADERS, json=data)
        response.raise_for_status()
        return response.json()
    except requests.RequestException as e:
        return {"error": str(e)}

def verify_otp(uid, code):
    url = f"{BASE_URL}/verify-otp"
    data = {"uid": uid, "code": code}
    try:
        response = requests.post(url, headers=HEADERS, json=data)
        response.raise_for_status()
        return response.json()
    except requests.RequestException as e:
        return {"error": str(e)}

# Example usage
phone = 256700123456
uid = "123e4567-e89b-12d3-a456-426614174000"
print(send_otp(phone, uid))  # Sends OTP
print(verify_otp(uid, "123456"))  # Verifies OTP
```

</details>

<details>
<summary>🔥 JavaScript Example</summary>

```javascript
const BASE_URL = "https://openlyne.sliplane.app/webhook";
const API_KEY = "YOUR_API_KEY";
const HEADERS = {
  "x-api-key": API_KEY,
  "Content-Type": "application/json"
};

async function sendOTP(phone, uid, env = "sandbox") {
  try {
    const response = await fetch(`${BASE_URL}/send-otp`, {
      method: "POST",
      headers: HEADERS,
      body: JSON.stringify({ phone, uid, env })
    });
    return await response.json();
  } catch (error) {
    return { error: error.message };
  }
}

async function verifyOTP(uid, code) {
  try {
    const response = await fetch(`${BASE_URL}/verify-otp`, {
      method: "POST",
      headers: HEADERS,
      body: JSON.stringify({ uid, code })
    });
    return await response.json();
  } catch (error) {
    return { error: error.message };
  }
}

// Example usage
const phone = 256700123456;
const uid = "123e4567-e89b-12d3-a456-426614174000";
sendOTP(phone, uid).then(console.log); // Sends OTP
verifyOTP(uid, "123456").then(console.log); // Verifies OTP
```

</details>

---

**Replace `YOUR_API_KEY` with your actual API key. You're ready to go! 🚀**
