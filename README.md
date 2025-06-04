# 🌐 Toshka API – Integration with Valmont Servers

## 🧩 Overview
This API allows **28 Valmont servers** to send POST requests to the **Toshka server** to update telemetry data (20 tags) for individual Pivots. Each Valmont server must authenticate and acquire a token before submitting data.

---

## 🔐 Authentication Flow

1. **Valmont → Toshka**: Authenticate using username and password.
2. **Toshka → Valmont**: Returns an **access token**.
3. **Valmont → Toshka**: Uses token + credentials to send tag updates for one Pivot.

---

## 📍 Endpoints

### 1. `POST /generateToken`
**Purpose**: Authenticates the Valmont server and returns an access token.

#### Request
```json
{
  "username": "valmont_user",
  "password": "your_password"
}
