# 🌐 API Workflow: Valmont ↔ Toshka ↔ Pivot Tags

This document explains how **Valmont servers** send **pivot data** to the **Toshka server**, which then updates the final records. It's made simple for both technical and non-technical users.

---

## 🧹 Step 1: Get Token (Authentication)

**🔸 Who:** Valmont 28 Servers
**🔸 What:** Request a token from Toshka to authenticate

### ➔ Request

**GET** `/generateToken`
**Headers:**

```
Authorization: Basic (username:password)
```

### ➔ Toshka Response

✅ Returns a **token** if credentials are valid

---

## 🚚 Step 2: Send Pivot Data

**🔸 Who:** Valmont
**🔸 What:** Send pivot status updates to Toshka
**🔸 How:** Make a **POST** request to `sendPivotStatus`

### ➔ Request

**POST** `/sendPivotStatus`
**Headers:**

```
Authorization: Bearer <token>
Username: <username>
Password: <password>
```

**Body (JSON):**

```json
[
  { "pivotName": "Pivot1", "status": "On", "pressure": 32.1 },
  { "pivotName": "Pivot2", "status": "Off", "pressure": 30.5 },
  ...
  20 pivot items
]
```

---

## 🧠 Step 3: Validation & Processing (Toshka Side)

Toshka performs the following:

✅ Token validation
✅ Header check (username & password)
✅ JSON body must have **exactly 20 items**
✅ Each item must contain required fields:

* `pivotName`
* `status`
* Other key values (like `pressure`)
  ✅ Type check (e.g. string, number, etc.)

---

## 📅 Step 4: Data Update

If all checks pass, Toshka:

* Updates each pivot entry in the system
* Stores latest data in the pivot tags database

Example update:

```
Pivot Data Storage
 ├── Pivot1 → status = On, pressure = 32.1
 ├── Pivot2 → status = Off, pressure = 30.5
 └── ... etc.
```

---

## 🔁 Visual Flow Summary

```mermaid
graph TD
  A[Valmont 28 Servers] -->|1. GET /generateToken| B[Toshka Server]
  B -->|Token Response| A
  A -->|2. POST /sendPivotStatus (20 items)| B
  B -->|3. Validate JSON & Headers| B
  B -->|4. Update Data| C[Pivot Tags Storage]
```

---

## ✅ Key Notes

* Use HTTPS for all requests
* Token expires after a set time (e.g. 30 min)
* Ensure pivot JSON includes all 20 records
* Toshka will reject requests missing `pivotName` or with wrong data types

---

📄 **Last Updated:** June 2025
🛠️ **Maintainer:** API Integration Team – Valmont/Toshka Systems
