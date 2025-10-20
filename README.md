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
  -F "serial_number=B8926E094702NXXXXX" \
  -F "purchase_date=2025-09-01" \
  -F "document_number=INV-555" \
  -F "merchant_name=Ηλεκτρομάγαζο"
```

## 💻 Code Examples

### Postman

#### 📥 Εισαγωγή Collection

1. Κατεβάστε το Postman collection: [Sendo Warranty API.postman_collection.json](https://github.com/Intercool-IT/warranties-api-doc/raw/main/Sendo%20Warranty%20API.postman_collection.json)
2. Ανοίξτε το **Postman**
3. Κάντε κλικ στο **Import** (πάνω αριστερά)
4. Επιλέξτε το αρχείο JSON που κατεβάσατε
5. Το collection θα εμφανιστεί στο αριστερό sidebar

#### 🚀 Χρήση

1. Επιλέξτε το request **"Register Warranty"**
2. Η διεύθυνση είναι ήδη ορισμένη: `https://sendo.world/api/register_warranty/`
3. Πηγαίνετε στο tab **Body**
4. Βεβαιωθείτε ότι είναι επιλεγμένο **form-data** (όχι raw/JSON)
5. Συμπληρώστε τα πεδία όπως φαίνεται στον πίνακα πεδίων
6. Κάντε κλικ **Send**

---

### PHP

```php
<?php
// PHP Example - Warranty Registration API Call

// API endpoint
$apiUrl = "https://sendo.world/api/register_warranty/index.php";

// Prepare form data
$formData = [
    "owner_name" => "Γιάννης Παπαδόπουλος",
    "address" => "Αθήνα 123",
    "postal_code" => "12345",
    "mobile_phone" => "+306912345678",
    "landline" => "2101234567",
    "email" => "giannis@example.com",
    "serial_number" => "B8926E094702NXXXXX",
    "purchase_date" => "2025-09-01",
    "document_number" => "INV-555",
    "merchant_name" => "Ηλεκτρομάγαζο"
];

// Initialize cURL
$ch = curl_init();

curl_setopt_array($ch, [
    CURLOPT_URL => $apiUrl,
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => http_build_query($formData),
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_TIMEOUT => 10,
    CURLOPT_SSL_VERIFYPEER => true
]);

// Execute request
$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

if (curl_errno($ch)) {
    echo "cURL Error: " . curl_error($ch);
    curl_close($ch);
    exit;
}

curl_close($ch);

// Decode response
$result = json_decode($response, true);

// Handle response
if ($httpCode === 201 && $result['status'] === 'success') {
    echo "✅ Warranty registered successfully!\n";
    echo "Warranty ID: " . $result['warranty_id'] . "\n";
    echo "SMS Status: " . $result['sms_status'] . "\n";
} else {
    echo "❌ Error: " . $result['message'] . "\n";
    echo "HTTP Code: " . $httpCode . "\n";
}

echo "\nFull Response:\n";
echo json_encode($result, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
?>
```

### JavaScript (Fetch API)

```javascript
// JavaScript Example - Warranty Registration API Call

const apiUrl = "https://sendo.world/api/register_warranty/index.php";

const formData = new FormData();
formData.append("owner_name", "Γιάννης Παπαδόπουλος");
formData.append("address", "Αθήνα 123");
formData.append("postal_code", "12345");
formData.append("mobile_phone", "+306912345678");
formData.append("landline", "2101234567");
formData.append("email", "giannis@example.com");
formData.append("serial_number", "B8926E094702NXXXXX");
formData.append("purchase_date", "2025-09-01");
formData.append("document_number", "INV-555");
formData.append("merchant_name", "Ηλεκτρομάγαζο");

fetch(apiUrl, {
    method: "POST",
    body: formData
})
.then(response => response.json())
.then(result => {
    if (result.status === "success") {
        console.log("✅ Warranty registered successfully!");
        console.log("Warranty ID:", result.warranty_id);
        console.log("SMS Status:", result.sms_status);
    } else {
        console.error("❌ Error:", result.message);
    }
    console.log("Full Response:", result);
})
.catch(error => {
    console.error("Request failed:", error);
});
```

### JavaScript (Async/Await)

```javascript
// JavaScript Async/Await Example

const registerWarranty = async () => {
    const apiUrl = "https://sendo.world/api/register_warranty/index.php";

    const formData = new FormData();
    formData.append("owner_name", "Γιάννης Παπαδόπουλος");
    formData.append("address", "Αθήνα 123");
    formData.append("postal_code", "12345");
    formData.append("mobile_phone", "+306912345678");
    formData.append("landline", "2101234567");
    formData.append("email", "giannis@example.com");
    formData.append("serial_number", "B8926E094702NXXXXX");
    formData.append("purchase_date", "2025-09-01");
    formData.append("document_number", "INV-555");
    formData.append("merchant_name", "Ηλεκτρομάγαζο");

    try {
        const response = await fetch(apiUrl, {
            method: "POST",
            body: formData
        });

        const result = await response.json();

        if (result.status === "success") {
            console.log("✅ Warranty registered successfully!");
            console.log("Warranty ID:", result.warranty_id);
            console.log("SMS Status:", result.sms_status);
        } else {
            console.error("❌ Error:", result.message);
        }

        return result;
    } catch (error) {
        console.error("Request failed:", error);
    }
};

// Call the function
registerWarranty();
```

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

> **Σημείωση:** Ακόμα και αν αποτύχει η **αποστολή/καταγραφή** του SMS, η εγγύηση καταχωρείται επιτυχώς και το SMS αποστέλλεται στον πελάτη.

### ⚠️ Error Responses

| HTTP Code | Status | Περιγραφή                                                    |
|-----------|--------|--------------------------------------------------------------|
| **400**       | error  | Λείπουν υποχρεωτικά πεδία ή invalid email format                |
| **404**       | error  | Ο σειριακός αριθμός δεν υπάρχει στη βάση δεδομένων(προς το παρών δεν επιστρέφει)             |
| **409**       | error  | Ο σειριακός αριθμός είναι ήδη καταχωρημένος                    |
| **405**       | error  | Το αίτημα δεν είναι POST                                       |
| **500**       | error  | Σφάλμα σύνδεσης βάσης δεδομένων                                |

### Παράδειγμα Error Response

```json
{
    "status": "error",
    "message": "Serial number already registered."
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