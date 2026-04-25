# Test-documentations
API-lər üçün test checklist-i, test ssenariləri və aşkarlanmış bug-lar.
# API Test Checklist

## 1. GET /applications — Müraciət siyahısı

### Test Case 1

**Expected Result:**

* Mövcud və qeydiyyatdan keçmiş user-ə aid müraciətlər siyahı şəklində qaytarılmalıdır.

```json
[
  {
    "id": "550e8400-...",
    "type": "My Passport",
    "status": "pending",
    "email": "user@example.com",
    "created_at": "2026-04-07T10:00:00Z"
  }
]
```

**Actual Result:**

```json
[
  {
    "id": "550e8400-...",
    "type": "My Passport",
    "status": "pending",
    "email": "user@example.com",
    "created_at": "2026-04-07T10:00:00Z"
  }
]
```

---

### Test Case 2 (Yanlış URL)

**Request:**

```
https://smplifai-backend.vercel.app/api/application
```

**Expected Result:**

* 400+ user error qaytarılmalıdır (yanlış URL)

**Actual Result:**

* 404 Not Found

---

## 2. POST /applications/submit — Tam müraciət göndər

### Test Case 1 (Düzgün data)

**Expected Result:**

* Yeni application yaradılmalıdır
* Status: 200/201 OK

```json
{
  "id": "...",
  "type": "My Passport",
  "status": "pending",
  "email": "john@example.com",
  "file_url": "https://...supabase.co/storage/..."
}
```

**Actual Result:**

* 201 OK — uğurlu yaradıldı

---

### Test Case 2 (Yanlış phone tipi)

**Input:**

* phone sahəsinə string daxil edildi

**Expected Result:**

* 400 Bad Request (data tipi səhvdir)

**Actual Result:**

* 201 OK — application yaradıldı ❌ BUG

---

### Test Case 3 (Tam data ilə real nümunə)

**Expected Result:**

* Yeni application yaradılmalıdır (201 OK)

**Actual Result:**

```json
{
  "id": "ac8ea0e7-564f-4cd3-9583-2af56f42b5d3",
  "user_id": "2ca021f4-c85d-418f-89e4-4aa38605f9f2",
  "type": "My Status",
  "email": "fəqan@example.com",
  "phone": "+994501234567",
  "passport_data": {
    "surname": "123",
    "firstName": "Fəqan"
  },
  "file_url": "https://tcrdbtaksnqezkbzgfys.supabase.co/storage/v1/object/public/passports/2ca021f4-c85d-418f-89e4-4aa38605f9f2-1776498615088.pdf",
  "status": "pending",
  "created_at": "2026-04-18T07:50:15.776005+00:00",
  "updated_at": "2026-04-18T07:50:15.776005+00:00"
}
```

---

## 3. POST /applications — Yeni müraciət yarat

### Test Case 1 (type daxil edilib)

**Expected Result:**

* 200 OK

**Actual Result:**

* 200 OK

---

### Test Case 2 (type daxil edilməyib)

**Expected Result:**

* 400 Bad Request

**Actual Result:**

```json
{
  "success": false,
  "message": "Unexpected token '}', \"{\n  \"type\":\n}\" is not valid JSON"
}
```

---

## 4. GET /applications/{id} — Tək müraciət

### Test Case 1 (doğru id)

**Expected Result:**

* 200 OK

**Actual Result:**

* 200 Created

---

### Test Case 2 (yanlış id)

**Input:**

* user_id istifadə edildi

**Expected Result:**

* 400+ Not Found

**Actual Result:**

* 404 Not Found

---

## 5. DELETE /applications/{id} — Müraciəti sil

### Test Case 1 (doğru id)

**Expected Result:**

* 204 No Content

**Actual Result:**

* 204 OK

---

### Test Case 2 (ID böyük hərflərlə)

**Expected Result:**

* 400+ Not Found

**Actual Result:**

* 204 OK ❌ BUG

---

## 6. PATCH /applications/{id}/status — Status dəyişdir

### Test Case 1 (status = "perfect")

**Expected Result:**

* 200 OK

**Actual Result:**

* 200 Created

---

### Test Case 2 (status boş)

**Expected Result:**

* 400 Bad Request

**Actual Result:**

* 400 Bad Request

---

## 7. PATCH /applications/{id}/passport — Pasport məlumatlarını yenilə

### Test Case 1 (doğru data)

**Expected Result:**

* 200 OK

**Actual Result:**

* 200 Created

---

### Test Case 2 (yanlış date format)

**Input:**

* "date of expiry" = "hello"

**Expected Result:**

* 400 Bad Request

**Actual Result:**

* 200 Created ❌ BUG

---

## 8. GET /applications/{id}/pdf — PDF yüklə

### Test Case

**Expected Result:**

* PDF faylı response body-də qaytarılmalıdır
* Status: 200 OK

**Actual Result:**

* PDF uğurla qaytarıldı
* 200 Created

---

# Ümumi Nəticə

### Aşkarlanan Problemlər:

* Phone sahəsində data tipi yoxlanılmır
* Passport tarix sahəsində validasiya yoxdur
* DELETE endpoint case-sensitive deyil
* Bəzi endpoint-lərdə status kodları düzgün qaytarılmır (Created vs OK)
