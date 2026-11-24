# HƯỚNG DẪN SỬ DỤNG THIỆP MỜI TỐT NGHIỆP ONLINE (BẢN NÂNG CẤP)
**Chủ nhân:** Trần Phạm Hoàng Trâm  
**Trường:** Trường Cao Đẳng Kinh Tế Đối Ngoại (COFER)

---

### 1. Các tính năng nổi bật trong bản mới:
1. **Màn hình mở thiệp siêu long lanh:**
   * Phong bì đổ bóng 3D, có dải ruy băng lụa vắt ngang.
   * Con dấu sáp vàng ánh kim phát sáng hào quang lấp lánh (pulsing gold seal).
   * Nút mở thiệp bo tròn viền vàng tỏa sáng với hiệu ứng hạt bụi vàng rơi nhẹ.
2. **Vòng xoay ảnh 3D (3D Carousel):**
   * Hiển thị toàn bộ **6 bức ảnh của Trâm** trong không gian 3 chiều.
   * Tự động xoay tròn mượt mà, có nút **Xoay trái / Xoay phải**, hoặc **vuốt tay trên màn hình điện thoại** để xoay ảnh.
   * Mũ cử nhân 3D bay tới đậu lên đầu bức ảnh phía trước kèm tia sáng lấp lánh!
3. **Tên uốn lượn nghệ thuật:**
   * Tên **Trần Phạm Hoàng Trâm** sử dụng font thư pháp nghệ thuật mềm mại (`Charm` & `Dancing Script`), hỗ trợ đầy đủ 100% dấu tiếng Việt.
4. **Form RSVP tinh gọn (Chỉ 3 mục theo đúng yêu cầu):**
   * Tên của bạn
   * Bạn sẽ đến cùng Trâm chứ? (🎉 Chắc chắn đến / 🥺 Tiếc quá, mình bận)
   * Lời nhắn / Lời chúc gửi Trâm

---

### 2. Kết nối Form RSVP về Google Sheets (3 mục)

#### Bước 1: Chuẩn bị Google Sheet
1. Mở [Google Sheets](https://sheets.google.com) tạo bảng tính mới (đặt tên: `Xác nhận tốt nghiệp - Hoàng Trâm`).
2. Dòng 1 điền đúng 4 tiêu đề cột sau:
   * **Cột A:** `Thời gian gửi`
   * **Cột B:** `Họ và tên`
   * **Cột C:** `Bạn sẽ đến cùng Trâm chứ?`
   * **Cột D:** `Lời chúc`

#### Bước 2: Dán mã Google Apps Script
1. Chọn **Tiện ích mở rộng (Extensions)** $\rightarrow$ **Apps Script**.
2. Xóa hết mã cũ và dán đoạn mã sau vào:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    // Ghi 4 cột tương ứng: Thời gian, Tên, Trạng thái tham gia, Lời chúc
    sheet.appendRow([
      data.timestamp || new Date().toLocaleString('vi-VN'),
      data.name || '',
      data.attendance || '',
      data.message || ''
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'success', message: 'OK' }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'error', message: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

#### Bước 3: Lấy link Web App
1. Bấm **Triển khai (Deploy)** $\rightarrow$ **Tùy chọn triển khai mới (New deployment)**.
2. Chọn loại: **Ứng dụng web (Web app)**.
   * **Thực thi dưới dạng:** `Tôi (Me)`
   * **Ai có quyền truy cập:** Chọn **Bất kỳ ai (Anyone)**.
3. Bấm **Triển khai (Deploy)** $\rightarrow$ Sao chép **URL ứng dụng web (Web App URL)**.
4. Mở file `index.html`, tìm biến `googleScriptUrl` dán link vừa copy vào là xong!
