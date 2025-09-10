# 📑 Warranty Registration API

API για καταχώρηση εγγυήσεων προϊόντων με στοιχεία πελάτη και αγοράς.

---

## 🔗 Endpoint

```
POST https://sendo.world/api/register_warranty/
Content-Type: application/json
```

---

## 📥 Request Body

| Πεδίο           | Τύπος  | Υποχρεωτικό | Περιγραφή                                      |
|-----------------|--------|-------------|------------------------------------------------|
| `owner_name`      | string | ✅ Ναι       | Ονοματεπώνυμο πελάτη                           |
| `address`         | string | ❌ Όχι       | Διεύθυνση πελάτη                               |
| `postal_code`     | string | ❌ Όχι       | Ταχυδρομικός κώδικας                           |
| `mobile_phone`    | string | ✅ Ναι       | Κινητό τηλέφωνο                                |
| `landline`        | string | ❌ Όχι       | Σταθερό τηλέφωνο                               |
| `email`           | string | ✅ Ναι       | Email πελάτη                                   |
| `serial_number`   | string | ✅ Ναι       | Σειριακός αριθμός προϊόντος                    |
| `purchase_date`   | string | ✅ Ναι       | Ημερομηνία αγοράς (μορφή YYYY-MM-DD)           |
| `document_number` | string | ✅ Ναι       | Αριθμός παραστατικού (π.χ. τιμολόγιο/απόδειξη) |
| `merchant_name`   | string | ✅ Ναι       | Επωνυμία καταστήματος                          |

---

## 📤 Παράδειγμα Αιτήματος

```http
POST https://sendo.world/api/register_warranty
Content-Type: application/json

{
  "owner_name": "Γιάννης Παπαδόπουλος",
  "address": "Αθήνα 123",
  "postal_code": "12345",
  "mobile_phone": "+306912345678",
  "landline": "2101234567",
  "email": "giannis@example.com",
  "serial_number": "SN123456789",
  "purchase_date": "2025-09-01",
  "document_number": "INV-555",
  "merchant_name": "Ηλεκτρομάγαζο"
}
```

---

## 📬 Responses

### ✅ Success (201 Created)

```json
{
  "status": "success",
  "message": "Warranty registered successfully.",
  "warranty_id": 123,
  "lifetime": true
}
```

### ⚠️ Error Responses

| HTTP Code | Status | Παράδειγμα Response                                                    |
|-----------|--------|------------------------------------------------------------------------|
| **400**       | error  | `{"status":"error","message":"Missing required fields."}`                |
| **404**       | error  | `{"status":"error","message":"Invalid serial number. Contact support."}` |
| **409**       | error  | `{"status":"error","message":"Serial number already registered."}`       |
| **405**       | error  | `{"status":"error","message":"Method not allowed. Use POST."}`           |
| **500**       | error  | `{"status":"error","message":"Database connection failed"}`              |

---

## 📌 Σημειώσεις

- Τα αιτήματα στέλνονται σε JSON.
- Οι σειριακοί αριθμοί ελέγχονται βάσει της επίσημης λίστας.
- Αν ο σειριακός αριθμός υπάρχει ήδη, επιστρέφεται `409 Conflict`.
- Με επιτυχημένη καταχώρηση αποστέλλεται αυτόματα SMS στο δηλωμένο κινητό.
- To `404 Error` θα γίνει rework ώστε να καταχωρεί το record απλα προς έγκριση, θα επιστρέφει ενα μήνυμα σχεδόν επιτυχίας και το sms θα φεύγει όταν το customer service μας επιβεβαιώσει το λάθος. Πρός ώρας να διαχειριστεί ως error, στο μέλλον θα θεωρείται **warning**.