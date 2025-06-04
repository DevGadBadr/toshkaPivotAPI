
# 🌐 API Workflow: Valmont ↔ Toshka ↔ Pivot Tags

> A clear, human-friendly overview of how Valmont servers communicate with Toshka to update pivot data.

---

## 🧩 Step 1: Get Token (Authentication)

**Who:** Valmont 28 Servers  
**What:** Request a token to access Toshka Server  
**How:** Send a GET request with Basic Auth credentials

```http
GET /generateToken
Authorization: Basic <base64(username:password)>
```

✅ If valid, Toshka returns a **token** for secure access.

---

## 🚚 Step 2: Send Pivot Data

**Who:** Valmont  
**What:** POST 20 pivot items to Toshka  
**How:** Send a POST request with token, username, and password

```http
POST /sendPivotStatus
Authorization: Bearer <token>
Username: valmontUser
Password: valmontPass
Content-Type: application/json
```

```json
[
  { "pivotName": "Pivot1", "status": "On", "pressure": 32.1 },
  { "pivotName": "Pivot2", "status": "Off", "pressure": 30.5 },
  ...
  20 total items
]
```

---

## 🧠 Step 3: Toshka Validates the Request

Toshka checks:

- ✅ Token is valid
- ✅ Username and password in headers
- ✅ Exactly 20 items in JSON
- ✅ Each item has:
  - `pivotName`
  - `status`
  - Correct data types (e.g., strings, numbers)

If anything is missing or incorrect → ❌ request is rejected.

---

## 🛠️ Step 4: Data Update

Once validated, Toshka updates the Pivot Tags:

```text
Pivot Tags:
├── Pivot1 → status: On, pressure: 32.1
├── Pivot2 → status: Off, pressure: 30.5
└── ... All 20 updated
```

---

## 🔁 Visual Flow Diagram

```mermaid
graph TD
  A[Valmont Servers] -->|GET /generateToken| B[Toshka Server]
  B -->|Returns Token| A
  A -->|POST /sendPivotStatus (20 items)| B
  B -->|Validate JSON & Auth| B
  B -->|Update Tags| C[Pivot Tag Storage]
```

---

## 📌 Key Points to Remember

- Use HTTPS for all requests.
- Token expires (e.g., after 30 minutes).
- Each request **must** contain 20 pivot items.
- `pivotName` is required for every item.
- Toshka performs both format and content validation.

---

📅 **Last Updated:** June 2025  
🔧 **Maintained by:** Valmont & Toshka API Integration Team
