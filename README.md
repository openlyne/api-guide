# Openlyne OTP

A simple and reliable SMS OTP (One-Time Password) service for JavaScript and Python applications. Built with [Africa's Talking](https://africastalking.com) SMS API for reliable message delivery across Africa and beyond.

## Features

- ✅ Send SMS OTP codes
- ✅ Verify OTP codes
- ✅ Test mode for development
- ✅ Production mode for live applications
- ✅ Simple API integration
- ✅ Error handling included
- ✅ Powered by Africa's Talking SMS API

## Quick Start

### JavaScript Setup

1. Create `otp.js` file:

```javascript
const API_KEY = 'your-api-key-here';

// Send OTP to a phone number
async function sendOTP(userID, phoneNumber, production) {
  const body = { uid: userID, phone: phoneNumber };
  if (production !== undefined) {
    body.production = production;
  }
  
  const response = await fetch('https://openlyne.sliplane.app/webhook/send-otp', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-API-Key': API_KEY,
    },
    body: JSON.stringify(body),
  });
  
  if (!response.ok) {
    throw new Error('Failed to send OTP');
  }
  
  return await response.json();
}

// Check if the OTP code is correct
async function verifyOTP(userID, otpCode) {
  const response = await fetch('https://openlyne.sliplane.app/webhook/verify-otp', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-API-Key': API_KEY,
    },
    body: JSON.stringify({
      uid: userID,
      code: otpCode,
    }),
  });
  
  if (!response.ok) {
    throw new Error('Failed to verify OTP');
  }
  
  return await response.json();
}

// Export functions to use in other files
module.exports = { sendOTP, verifyOTP };
```

2. Use in your application:

```javascript
const { sendOTP, verifyOTP } = require('./otp.js');

async function handleOTP() {
  try {
    // Send OTP (test mode)
    await sendOTP('user123', '256700000000');
    console.log('Test OTP sent!');
    
    // Send OTP (production mode)
    await sendOTP('user123', '256700000000', true);
    console.log('Production OTP sent!');
    
    // Verify OTP
    await verifyOTP('user123', '123456');
    console.log('OTP verified!');
    
  } catch (error) {
    console.log('Error:', error.message);
  }
}
```

### Python Setup

1. Install dependencies:
```bash
pip install requests
```

2. Create `otp.py` file:

```python
import requests

API_KEY = 'your-api-key-here'

def send_otp(user_id, phone_number, production=None):
    """Send OTP to a phone number"""
    body = {'uid': user_id, 'phone': phone_number}
    if production is not None:
        body['production'] = production
    
    response = requests.post(
        'https://openlyne.sliplane.app/webhook/send-otp',
        headers={
            'Content-Type': 'application/json',
            'X-API-Key': API_KEY,
        },
        json=body
    )
    
    if not response.ok:
        raise Exception('Failed to send OTP')
    
    return response.json()

def verify_otp(user_id, otp_code):
    """Check if the OTP code is correct"""
    response = requests.post(
        'https://openlyne.sliplane.app/webhook/verify-otp',
        headers={
            'Content-Type': 'application/json',
            'X-API-Key': API_KEY,
        },
        json={
            'uid': user_id,
            'code': otp_code,
        }
    )
    
    if not response.ok:
        raise Exception('Failed to verify OTP')
    
    return response.json()
```

3. Use in your application:

```python
from otp import send_otp, verify_otp

def handle_otp():
    try:
        # Send OTP (test mode)
        send_otp('user123', '256700000000')
        print('Test OTP sent!')
        
        # Send OTP (production mode)
        send_otp('user123', '256700000000', True)
        print('Production OTP sent!')
        
        # Verify OTP
        verify_otp('user123', '123456')
        print('OTP verified!')
        
    except Exception as error:
        print(f'Error: {error}')

if __name__ == '__main__':
    handle_otp()
```

## API Reference

### `sendOTP(userID, phoneNumber, production)`

Sends an OTP code to the specified phone number via Africa's Talking SMS API.

**Parameters:**
- `userID` (string): Unique identifier for the user
- `phoneNumber` (string): Phone number in international format
- `production` (boolean, optional): Set to `true` for real SMS, leave empty for test mode

**Returns:** Promise/dict with API response

### `verifyOTP(userID, otpCode)`

Verifies the OTP code entered by the user.

**Parameters:**
- `userID` (string): Same identifier used in `sendOTP`
- `otpCode` (string): The OTP code to verify

**Returns:** Promise/dict with verification result

## Usage Examples

### Test Mode (Default)
```javascript
// JavaScript
await sendOTP('user123', '256700000000');

# Python
send_otp('user123', '256700000000')
```

### Production Mode
```javascript
// JavaScript
await sendOTP('user123', '256700000000', true);

# Python
send_otp('user123', '256700000000', True)
```

### Verify OTP
```javascript
// JavaScript
await verifyOTP('user123', '123456');

# Python
verify_otp('user123', '123456')
```

## Phone Number Format

Always use international format (supported by Africa's Talking):
- **Uganda**: `256700000000`
- **Kenya**: `254700000000`
- **Tanzania**: `255700000000`
- **Rwanda**: `250700000000`
- **Nigeria**: `234700000000`
- **Ghana**: `233700000000`
- **South Africa**: `27700000000`
- **US**: `12345678901`
- **UK**: `447700000000`

## Testing

For testing, use the default mode (no `production` parameter). Test messages will appear in the Africa's Talking simulator:

🔗 **Test Simulator**: https://simulator.africastalking.com/

## Error Handling

Both JavaScript and Python implementations include proper error handling:

```javascript
// JavaScript
try {
  await sendOTP('user123', '256700000000');
} catch (error) {
  console.log('Error:', error.message);
}
```

```python
# Python
try:
    send_otp('user123', '256700000000')
except Exception as error:
    print(f'Error: {error}')
```

## Common Issues

| Issue | Solution |
|-------|----------|
| "Failed to send OTP" | Check API key and phone number format |
| "Failed to verify OTP" | Ensure same userID for send and verify |
| Network errors | Check internet connection |
| OTP expired | OTP codes expire after a few minutes |
| SMS not delivered | Check if phone number is supported by Africa's Talking |

## Project Structure

```
your-project/
├── otp.js (or otp.py)     # OTP functions
├── app.js (or app.py)     # Your main application
└── README.md              # This file
```

## API Endpoints

- **Send OTP**: `POST https://openlyne.sliplane.app/webhook/send-otp`
- **Verify OTP**: `POST https://openlyne.sliplane.app/webhook/verify-otp`

## SMS Provider

This service uses [Africa's Talking](https://africastalking.com) SMS API for reliable message delivery. Africa's Talking provides excellent coverage across Africa and supports international messaging.

## Support

For issues and questions, please create an issue in this repository.

## License

MIT License - see LICENSE file for details.

---

Made with ❤️ by the Openlyne team | Powered by [Africa's Talking](https://africastalking.com) SMS API
