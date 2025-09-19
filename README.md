# 🧩 Taxonomy Learning Games

Game interaktif untuk belajar **klasifikasi makhluk hidup** (Biologi SMA).  
Terdiri dari dua jenis permainan:

1. **MCQ (Pilihan Ganda)** — 30 soal dibagi 3 level.  
2. **Drag & Drop** — 20 set, susun urutan taksonomi dari Domain → Spesies.  

---

## 🚀 Cara Pakai

### 1. Setup Google Sheets
1. Buat spreadsheet baru → beri nama misalnya `Taxonomy Results`.  
2. Buka menu **Extensions → Apps Script**.  
3. Hapus isi default, lalu paste kode berikut:

```javascript
function doPost(e) {
  try {
    var ss = SpreadsheetApp.openById("YOUR_SHEET_ID_HERE"); // ganti dengan ID sheet Anda
    var sheet = ss.getSheetByName("responses") || ss.insertSheet("responses");
    var payload = e.postData.contents;
    var data = JSON.parse(payload);

    var ts = new Date();
    var row = [
      ts,
      data.name || "",
      data.class || "",
      data.absen || "",
      data.game || "",
      data.score || 0,
      data.duration || "",
      JSON.stringify(data.details || {})
    ];
    sheet.appendRow(row);

    return ContentService.createTextOutput(JSON.stringify({status:"success"}))
                         .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({status:"error", message:err.toString()}))
                         .setMimeType(ContentService.MimeType.JSON);
  }
}
