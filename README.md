# 📑 Warranty Registration API

API για καταχώρηση εγγυήσεων προϊόντων με στοιχεία πελάτη και αγοράς.

---

## 🔗 Endpoint

```
POST https://sendo.world/api/register_warranty/
Content-Type: multipart/form-data
```

---

## 📥 Request Body (Form Data)

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

### cURL

```bash
curl -X POST https://sendo.world/api/register_warranty/index.php \
  -F "owner_name=Γιάννης Παπαδόπουλος" \
  -F "address=Αθήνα 123" \
  -F "postal_code=12345" \
  -F "mobile_phone=+306912345678" \
  -F "landline=2101234567" \
  -F "email=giannis@example.com" \
  -F "serial_number=B8926E094702N00167" \
  -F "purchase_date=2025-09-01" \
  -F "document_number=INV-555" \
  -F "merchant_name=Ηλεκτρομάγαζο"
```

### Postman

1. Method: **POST**
2. URL: `https://sendo.world/api/register_warranty/index.php`
3. Body: **form-data** (όχι raw/JSON)
4. Συμπληρώστε τα πεδία όπως φαίνεται στον πίνακα πεδίων

---

## 📬 Responses

### ✅ Success - Warranty & SMS Sent (201 Created)

```json
{
  "status": "success",
  "message": "Warranty registered successfully and SMS sent.",
  "warranty_id": 123,
  "lifetime": true,
  "sms_status": "sent",
  "gateway_message_id": 98765
}
```

### ✅ Success - Warranty Registered but SMS Logging Failed (201 Created)

```json
{
  "status": "success",
  "message": "Warranty registered successfully but SMS logging failed.",
  "warranty_id": 123,
  "lifetime": true,
  "sms_status": "failed",
  "sms_error": "SMS log insert failed: [database error]"
}
```

> **Σημείωση:** Ακόμα και αν αποτύχει η καταγραφή του SMS, η εγγύηση καταχωρείται επιτυχώς και το SMS αποστέλλεται στον πελάτη.

### ⚠️ Error Responses

| HTTP Code | Status | Περιγραφή                                                    |
|-----------|--------|--------------------------------------------------------------|
| **400**       | error  | Λείπουν υποχρεωτικά πεδία ή invalid email format                |
| **404**       | error  | Ο σειριακός αριθμός δεν υπάρχει στη βάση δεδομένων             |
| **409**       | error  | Ο σειριακός αριθμός είναι ήδη καταχωρημένος                    |
| **405**       | error  | Το αίτημα δεν είναι POST                                       |
| **500**       | error  | Σφάλμα σύνδεσης βάσης δεδομένων                                |

### Παράδειγμα Error Response

```json
{
  "status": "error",
  "message": "Invalid serial number. Contact support."
}
```

---

## 🔔 SMS Notification

Με επιτυχημένη καταχώρηση της εγγύησης, το σύστημα αποστέλλει αυτόματα SMS στο δηλωμένο κινητό τηλέφωνο με:
- Ευχαριστίες για την ενεργοποίηση της εγγύησης
- Link προς τους όρους εγγύησης (https://sendo.gr)
- Πληροφορίες για νέα προϊόντα

---

## 📌 Σημειώσεις

- Τα αιτήματα αποστέλλονται ως **form-data** (multipart/form-data), **όχι** JSON.
- Οι σειριακοί αριθμοί ελέγχονται βάσει της επίσημης λίστας στη βάση δεδομένων.
- Αν ο σειριακός αριθμός υπάρχει ήδη, επιστρέφεται `409 Conflict` και δεν καταχωρείται νέα εγγύηση.
- Το κινητό τηλέφωνο αποδεκτό σε μορφές: `+306912345678` ή `00306912345678`.
- Η ημερομηνία αγοράς πρέπει να είναι σε μορφή `YYYY-MM-DD`.
- Τα αιτήματα επιστρέφουν πάντα JSON response με κατάλληλο HTTP status code.
- **Σχεδιαζόμενη βελτίωση:** Το 404 error θα rework ώστε να καταχωρεί το record προς έγκριση, να επιστρέφει quasi-success message και το SMS να φεύγει όταν το customer service επιβεβαιώσει. Πρός ώρας θα διαχειρίζεται ως error, στο μέλλον θα είναι **warning**.