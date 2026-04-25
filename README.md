# Test-documentations
API-lər üçün test checklist-i, test ssenariləri və aşkarlanmış bug-lar.
# API Test Ssenariləri

## 1. GET /applications — Müraciət siyahısı

### Test Case 1
**Expected Result:**
- Mövcud user-ə aid müraciətlər siyahı şəklində gəlməlidir.

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
