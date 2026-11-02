# 📋 BẢNG PHÂN CÔNG NHIỆM VỤ KIỂM THỬ E2E TOÀN DIỆN & HƯỚNG DẪN HỌC KIẾN TRÚC HỆ THỐNG
> **Ngày triển khai**: 02/11/2026  
> **Dự án**: Smart StudyHub RAG - Nền tảng Tự học Cá nhân hóa Đa môn học & Trợ lý AI Giáo trình  
> **Học phần**: HSF302 - Java Web Application & Spring Framework (FPT University)  
> **Đội ngũ thực hiện**: Nhóm 2 (Group 2)  
> **Nhánh Git chung**: `main`  
> **Tài liệu theo dõi lỗi**: [`docs/issues/ISSUES-TRACKING.md`](../issues/ISSUES-TRACKING.md)  
> **Thư mục lưu ảnh minh chứng**: `docs/evidence/`

---

## 🎯 1. MỤC TIÊU & NGUYÊN TẮC KIỂM THỬ E2E (END-TO-END)

1. **Mục tiêu**: Rà soát 100% chức năng, toàn bộ nút bấm, form nhập liệu, modal, các trường hợp thành công (happy case) và thất bại (edge/adversarial case), kiểm soát hạn ngạch 3 gói cước (**FREE**, **PRO**, **SQUAD**) trước thềm thuyết trình và chấm bài đồ án.
2. **Nguyên tắc phân công (Zero Conflict)**:
   - Mỗi thành viên phụ trách **1 phân hệ chuyên biệt** và kiểm thử trên **tài khoản demo độc lập** được hệ thống chuẩn bị sẵn.
   - Không ai giẫm chân lên dữ liệu của nhau (áp dụng cơ chế Data Isolation).
3. **Bắt buộc chụp ảnh màn hình (Screenshot Evidence)**:
   - Mỗi test case hoàn thành **bắt buộc phải có 1 ảnh chụp màn hình rõ nét** minh chứng kết quả (thành công hoặc bị chặn đúng quy tắc).
   - Đặt tên file ảnh theo cú pháp: `[Mã_Test]_[Tên_Thành_Viên].png` (Ví dụ: `E2E-CORE-01_tung.png`, `E2E-RAG-04_binh.png`).
   - Lưu toàn bộ ảnh vào thư mục `docs/evidence/` và commit lên repo.
4. **Quy trình khi phát hiện lỗi**:
   - Nếu nút bấm không hoạt động, giao diện bị vỡ, hoặc logic hạn ngạch không chặn đúng: **Mở ngay file [`docs/issues/ISSUES-TRACKING.md`](../issues/ISSUES-TRACKING.md)**, ghi nhận theo mẫu issue để phân công sửa lỗi ngay lập tức.

---

## 💡 2. MẸO KIỂM THỬ HẠN NGẠCH NHANH (FAST TESTING HACKS)
> [!TIP]
> **Làm thế nào để test hạn ngạch mà không mất hàng giờ đồng hồ bấm tay?** Hãy áp dụng các mẹo thông minh sau:

* **Mẹo 1: Test Hạn ngạch 50 câu RAG/ngày của Gói Free**:
  - Không cần ngồi chat đủ 50 câu! Bạn có 2 cách cực nhanh:
    - **Cách A (Khuyên dùng)**: Mở file `src/main/java/com/group2/assignment/rag/service/LlmKeyRotator.java` (hoặc Controller), tạm thời chỉnh hằng số `FREE_DAILY_RAG_LIMIT = 3;` (thay vì 50). Khởi động lại app, dùng tài khoản `student` chat 3 câu, đến câu thứ 4 hệ thống hiện thông báo màu đỏ chặn lại ngay! *(Sau khi test chụp ảnh xong, nhớ sửa lại thành 50)*.
    - **Cách B (Dùng H2 Console)**: Truy cập `http://localhost:8080/h2-console`, chạy câu lệnh SQL:
      ```sql
      UPDATE users SET daily_rag_count = 49 WHERE username = 'student';
      ```
      Sau đó vào web chat đúng 1 câu nữa là đạt 50/50! Câu tiếp theo sẽ bị chặn ngay lập tức!
* **Mẹo 2: Test Hạn ngạch 3 môn học của Gói Free**:
  - Tài khoản `student` đã được seed sẵn **2 môn học** (`HSF302`, `MAS291`).
  - Bạn chỉ cần bấm "Tạo môn học mới" thêm 1 môn thứ 3 (ví dụ `SWP391`) -> Tạo thành công!
  - Bấm tiếp tạo môn thứ 4 -> Nút tạo môn bị disable hoặc popup báo lỗi: *"Bạn đã đạt giới hạn 3 môn của Gói Free"*. Xóa 1 môn cũ đi thì nút tạo mở lại ngay!
* **Mẹo 3: Test Hạn ngạch 5 thành viên của Gói Study Squad**:
  - Tài khoản `squad_leader` đã có sẵn 3 bạn trong nhóm (`squad_leader`, `squad_member1`, `squad_member2`).
  - Bạn chỉ cần nhập `@student2` mời thêm -> Nhóm lên 4 bạn.
  - Tạo nhanh 1 user hoặc mời thêm 1 bạn nữa cho đủ 5 bạn. Khi nhóm đủ 5 bạn, ô nhập mời thành viên tự động bị khóa và báo *"Nhóm đã đủ tối đa 5 thành viên"*.
* **Mẹo 4: Đăng nhập nhanh 1-Click**:
  - Tại trang `http://localhost:8080/login`, bên dưới form có sẵn các nút bấm:
    `[FREE: student]`, `[PRO: pro_student]`, `[SQUAD: squad_leader]`, `[ADMIN: admin]`.
  - Chỉ cần bấm vào huy hiệu là hệ thống tự điền username và password `123456`, bấm Enter là vào ngay!

---

## 📖 3. HƯỚNG DẪN HỌC & HIỂU SÂU HỆ THỐNG (ĐỂ TRẢ LỜI VẤN ĐÁP GIẢNG VIÊN)

Mọi thành viên cần nắm vững 5 nguyên lý kiến trúc sau để tự tin trả lời vấn đáp:

1. **Kiến trúc Package-by-Feature (Theo tính năng)**:
   - Không chia package theo kiểu cũ (tất cả controller chung 1 chỗ), mà mỗi module nằm trọn vẹn trong package của nó:
     + `core`: Quản lý môn học, trang chủ, cách ly dữ liệu.
     + `auth`: Đăng ký, đăng nhập JWT cookie, đổi mật khẩu, quản lý nhóm Study Squad.
     + `rag`: Trích xuất PDF, cắt nhỏ chunks, embedding vector, cosine similarity, chatbot giáo trình.
     + `quiz`: Sinh đề AI, phòng thi split-view, chấm điểm, xuất đề thi PDF có đáp án & watermark, squad leaderboard.
     + `planner`: Lập kế hoạch học tập cá nhân và phân công đồ án nhóm (Team Tasks) theo Spaced Repetition.
     + `settings`: Trung tâm quản trị Admin Command Center, cấu hình API key, thống kê 3 gói cước.
2. **Cơ chế Phân lập Dữ liệu (Data Isolation) bằng `user_id`**:
   - `Course.java` liên kết `@ManyToOne User user`.
   - Khi truy vấn môn học, `CourseRepository` chạy câu lệnh SQL lọc `WHERE c.user.id = :userId`.
   - Nhờ đó, tài khoản `student` và `pro_student` đều có thể tự tạo môn `HSF302` cho riêng mình mà không bị lỗi trùng mã code toàn cục và không bao giờ nhìn thấy môn của nhau. Admin có quyền xem tất cả.
3. **Cơ chế RAG AI 0% Ảo giác (Retrieval-Augmented Generation)**:
   - Giáo trình PDF được chia nhỏ thành các đoạn chunk (~1000 ký tự có 200 ký tự overlap).
   - Khi sinh viên hỏi câu hỏi, hệ thống chuyển câu hỏi thành vector, so khớp Cosine Similarity với các chunk trên H2 Database để chọn ra 3-5 đoạn sát nhất.
   - Prompt gửi lên Gemini ép buộc: *"Chỉ được trả lời dựa trên các trích dẫn tài liệu đính kèm, nếu không có phải báo không có trong giáo trình, kèm số trang trích dẫn"*. Do đó AI không bị ảo giác bịa thông tin.
4. **Cơ chế Feature Gating 3 Gói Cước**:
   - Mọi API và Service đều kiểm tra gói cước người dùng thông qua các phương thức tiện ích trên `User.java`:
     + `user.isPro()`: Trả về `true` nếu là `PRO`, `SQUAD` hoặc `ADMIN`.
     + `user.isSquad()`: Trả về `true` nếu là `SQUAD`.
     + `user.isSquadLeader()`: Trả về `true` nếu là chủ sở hữu nhóm Study Squad.
   - Nếu gói Free cố tình gọi API tính năng Pro (như xuất PDF quiz, mở Study Planner, tạo môn thứ 4), Backend ném `IllegalStateException` / `AccessDeniedException` ngay lập tức.
5. **Cơ sở dữ liệu H2 Nhúng & Quản lý bằng H2 Console**:
   - Database lưu trữ file tại `./data/studyhub.mv.db` với chế độ `AUTO_SERVER=TRUE`.
   - Xem dữ liệu bảng trực tiếp tại: `http://localhost:8080/h2-console` (JDBC URL: `jdbc:h2:file:./data/studyhub;AUTO_SERVER=TRUE`, User: `sa`, Pass để trống).

---

## 👥 4. BẢNG PHÂN CHIA NHIỆM VỤ TEST E2E CHI TIẾT (6 THÀNH VIÊN)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       SƠ ĐỒ PHÂN BỔ NHIỆM VỤ KIỂM THỬ                       │
├─────────────────┬──────────────────────┬────────────────────────────────────┤
│ Thành viên      │ Phân hệ phụ trách    │ Tài khoản Test chính               │
├─────────────────┼──────────────────────┼────────────────────────────────────┤
│ 👑 TÙNG (Lead)  │ Core & Data Isolation│ student (Free), pro_student (Pro)  │
│ 👤 TRƯỜNG       │ Auth, Profile & Squad│ squad_leader, student2, member1    │
│ 📄 BÌNH         │ Documents & RAG AI   │ student (Free), pro_student (Pro)  │
│ 🧠 THÀNH        │ AI Quiz & Leaderboard│ squad_leader, squad_member1        │
│ 🖨️ NHẬT         │ Quiz PDF Export & UI │ student (Free), pro_student (Pro)  │
│ 🛡️ AN           │ Admin Command Center │ admin (Admin), student (Chặn 403)  │
└─────────────────┴──────────────────────┴────────────────────────────────────┘
```

---

### 👑 1. TÙNG (LEAD) - CORE, SEARCH/FILTER, DATA ISOLATION & HẠN MỨC 3 MÔN
* **Tài khoản sử dụng**: `student` (Free) và `pro_student` (Pro)
* **URL kiểm thử**: `http://localhost:8080/courses`, `http://localhost:8080/`
* **Danh sách Test Cases**:
  1. `E2E-CORE-01`: **Tìm kiếm môn học theo từ khóa**:
     - Nhập từ khóa `"Spring"` vào thanh tìm kiếm -> Kết quả phải lọc ra đúng môn `HSF302` (JPA and Spring Framework).
     - Nhập từ khóa không tồn tại `"XYZ123"` -> Hiển thị thông báo *"Không tìm thấy môn học nào phù hợp"*.
  2. `E2E-CORE-02`: **Lọc môn học theo học kỳ**:
     - Chọn dropdown Học kỳ `"Fall 2026"` -> Hiển thị danh sách môn thuộc kỳ Fall 2026.
     - Chọn `"Tất cả học kỳ"` -> Hiển thị lại toàn bộ môn.
  3. `E2E-CORE-03`: **Data Isolation (Phân lập dữ liệu môn học cá nhân)**:
     - Đăng nhập `student`: Chỉ nhìn thấy các môn của `student` (`HSF302`, `MAS291`). Không thấy môn của `pro_student` (`FER202`, `SWE201`...).
     - Đăng nhập `pro_student`: Chỉ nhìn thấy 4 môn của `pro_student`. Tuyệt đối không lẫn lộn!
  4. `E2E-CORE-04`: **Kiểm soát hạn mức 3 môn học gói Free**:
     - Tài khoản `student` đang có 2 môn. Bấm "Tạo môn học mới", tạo môn thứ 3 (Code: `SWP391`, Tên: `Software Development Project`) -> Tạo thành công.
     - Tiếp tục bấm tạo môn thứ 4 (Code: `PRJ301`) -> Hệ thống chặn lại, hiển thị thông báo lỗi đạt giới hạn 3 môn của Gói Free hoặc mở Modal Bảng giá.
     - Xóa 1 môn cũ -> Nút tạo môn học mở lại, tạo được ngay.
  5. `E2E-CORE-05`: **Gói Pro tạo môn học không giới hạn**:
     - Đăng nhập `pro_student` (đã có 4 môn), bấm tạo môn thứ 5 (`CSD201`) -> Tạo thành công mượt mà, không hề bị chặn!
  6. `E2E-CORE-06`: **Bảo vệ quyền sở hữu (Ownership Protection)**:
     - Đăng nhập `student`, cố tình nhập URL sửa/xóa môn học thuộc sở hữu của `pro_student` (ví dụ `/courses/edit/3`) -> Hệ thống chặn lại báo lỗi quyền truy cập.
* **Bằng chứng cần nộp**: 6 ảnh chụp màn hình tương ứng (`E2E-CORE-01_tung.png` đến `E2E-CORE-06_tung.png`).

---

### 👤 2. TRƯỜNG - AUTHENTICATION, PROFILE & QUẢN LÝ NHÓM SQUAD
* **Tài khoản sử dụng**: `squad_leader`, `student2`, `squad_member1`, `pro_student`
* **URL kiểm thử**: `http://localhost:8080/login`, `http://localhost:8080/profile`
* **Danh sách Test Cases**:
  1. `E2E-AUTH-01`: **Đăng nhập 1-Click tự động**:
     - Mở trang `/login`, bấm vào huy hiệu `[SQUAD: squad_leader]` -> Cả 2 ô username và password tự điền đúng -> Bấm Đăng nhập -> Vào thẳng trang chủ.
  2. `E2E-AUTH-02`: **Cập nhật họ tên & Avatar**:
     - Tại trang `/profile`, đổi họ tên và bấm chọn avatar Robot mới -> Bấm "Lưu Cập Nhật" -> Ảnh avatar và họ tên ở Header đổi ngay lập tức, có flash alert xanh.
  3. `E2E-AUTH-03`: **Đổi mật khẩu bảo mật BCrypt**:
     - Thử nhập sai mật khẩu cũ -> Báo lỗi.
     - Nhập đúng mật khẩu cũ `123456`, mật khẩu mới `654321` -> Đổi thành công. Đăng xuất ra đăng nhập lại bằng mật khẩu mới thành công (sau đó đổi lại `123456` để tiện demo).
  4. `E2E-AUTH-04`: **Trưởng nhóm mời bạn học vào Study Squad**:
     - Đăng nhập `squad_leader` vào `/profile`.
     - Nhập username `student2` vào ô "Mời Bạn Bè Vào Nhóm" -> Bấm Thêm -> Danh sách thành viên tăng lên 4 bạn, `student2` xuất hiện trong bảng.
  5. `E2E-AUTH-05`: **Chặn thêm thành viên khi nhóm đã đủ 5 bạn**:
     - Mời thêm 1 bạn nữa cho đủ 5/5 thành viên -> Ô nhập bị disable và hiển thị thông báo *"Nhóm đã đủ tối đa 5 thành viên"*.
  6. `E2E-AUTH-06`: **Xóa thành viên khỏi nhóm**:
     - Bấm nút "Xóa" cạnh `student2` -> Xác nhận confirm popup -> `student2` bị xóa khỏi nhóm, quay về gói Free.
  7. `E2E-AUTH-07`: **Thành viên nhóm hưởng quyền lợi Pro & Kiểm tra không lỗi @null**:
     - Đăng nhập `squad_member1` vào `/profile` -> Kiểm tra thông báo: *"Bạn đang là thành viên trong Nhóm học tập của Trưởng nhóm @squad_leader"*, tuyệt đối KHÔNG có chữ `@null`.
     - Đăng nhập `pro_student` vào `/profile` -> Kiểm tra thẻ Quản lý nhóm Study Squad **hoàn toàn biến mất**, chỉ hiển thị quyền lợi Pro cá nhân!
* **Bằng chứng cần nộp**: 7 ảnh chụp màn hình (`E2E-AUTH-01_truong.png` đến `E2E-AUTH-07_truong.png`).

---

### 📄 3. BÌNH - QUẢN LÝ TÀI LIỆU, CẮT CHUNKS, RAG CHATBOT & QUOTA RAG
* **Tài khoản sử dụng**: `student` (Free) và `pro_student` (Pro)
* **URL kiểm thử**: `http://localhost:8080/courses/1/documents`, `http://localhost:8080/courses/1/chat`
* **Danh sách Test Cases**:
  1. `E2E-RAG-01`: **Tải lên tài liệu PDF môn học**:
     - Vào môn học `HSF302`, tải file slide PDF lên -> Hệ thống xử lý trích xuất văn bản và hiển thị tài liệu trong danh sách.
  2. `E2E-RAG-02`: **Cắt nhỏ Chunks & Vector Search**:
     - Kiểm tra số lượng Chunks được tạo ra. Bấm xem chi tiết đoạn chunk: nội dung rõ ràng, có chỉ số trang chính xác.
  3. `E2E-RAG-03`: **Chatbot RAG Giáo trình có Citation số trang**:
     - Đặt câu hỏi chuyên sâu về bài học (ví dụ: *"Spring Data JPA là gì?"* hoặc nội dung có trong PDF).
     - AI phản hồi mạch lạc, bên dưới câu trả lời có **Citation Card ghi rõ: Nguồn file PDF + Trang X đối chứng**.
  4. `E2E-RAG-04`: **Kiểm soát hạn mức 3 tài liệu PDF ở Gói Free**:
     - Tài khoản `student` tải đủ 3 tài liệu PDF vào môn học.
     - Cố tình tải tài liệu thứ 4 -> Hệ thống chặn lại và hiển thị thông báo lỗi hạn ngạch 3 tài liệu/môn của Gói Free.
  5. `E2E-RAG-05`: **Gói Pro tải tài liệu PDF không giới hạn**:
     - Đăng nhập `pro_student`, tải liên tiếp 4-5 tài liệu PDF vào môn học -> Tải lên thành công mượt mà!
  6. `E2E-RAG-06`: **Kiểm tra Hạn ngạch RAG 50 câu/ngày của Gói Free**:
     - Áp dụng **Mẹo 1** (chỉnh hạn mức về 3 hoặc update SQL `daily_rag_count = 49`).
     - Gửi câu hỏi chat -> Hệ thống chặn lại và thông báo: *"Bạn đã đạt hạn ngạch 50 câu hỏi RAG hôm nay của Gói Free. Vui lòng nâng cấp Pro để tiếp tục"*.
* **Bằng chứng cần nộp**: 6 ảnh chụp màn hình (`E2E-RAG-01_binh.png` đến `E2E-RAG-06_binh.png`).

---

### 🧠 4. THÀNH - SINH ĐỀ THI TRẮC NGHIỆM AI, PHÒNG THI & SQUAD LEADERBOARD
* **Tài khoản sử dụng**: `squad_leader` và `squad_member1`
* **URL kiểm thử**: `http://localhost:8080/courses/1/quiz`, `http://localhost:8080/quiz`
* **Danh sách Test Cases**:
  1. `E2E-QUIZ-01`: **Sinh đề thi trắc nghiệm AI từ slide PDF**:
     - Chọn số lượng câu hỏi (ví dụ 5 câu hoặc 10 câu), bấm "Tạo Đề Thi Bằng AI" -> AI đọc nội dung giáo trình PDF và sinh đề thi trắc nghiệm 4 đáp án A/B/C/D bám sát kiến thức.
  2. `E2E-QUIZ-02`: **Giao diện phòng thi Split-view & Đếm ngược thời gian**:
     - Bấm "Bắt đầu làm bài" -> Giao diện phòng thi mở ra với đồng hồ đếm ngược, điều hướng danh sách câu hỏi mượt mà, lưu trạng thái câu đã chọn.
  3. `E2E-QUIZ-03`: **Nộp bài & Chấm điểm tức thì kèm giải thích chi tiết**:
     - Chọn đáp án cho các câu và bấm "Nộp Bài" -> Hệ thống chấm điểm ngay lập tức (ví dụ: 8.0/10), hiển thị đáp án đúng màu xanh, câu sai màu đỏ kèm đoạn văn bản giải thích trích xuất từ slide.
  4. `E2E-QUIZ-04`: **Lưu lịch sử bài làm**:
     - Quay lại trang danh sách đề thi -> Lịch sử lần làm bài vừa hoàn thành hiển thị đầy đủ ngày giờ, số câu đúng và điểm số.
  5. `E2E-QUIZ-05`: **Bảng xếp hạng nhóm (Squad Leaderboard)**:
     - Tài khoản `squad_leader` làm bài đạt 8.0 điểm.
     - Đăng nhập `squad_member1` làm cùng đề thi đó đạt 10.0 điểm.
     - Vào trang kết quả -> Bảng xếp hạng nhóm hiển thị `squad_member1` xếp hạng 1, `squad_leader` xếp hạng 2, thể hiện sự thi đua học tập trong nhóm SQUAD.
* **Bằng chứng cần nộp**: 5 ảnh chụp màn hình (`E2E-QUIZ-01_thanh.png` đến `E2E-QUIZ-05_thanh.png`).

---

### 🖨️ 5. NHẬT - XUẤT ĐỀ THI PDF, FONT TIẾNG VIỆT & FEATURE GATING TẢI ĐỀ
* **Tài khoản sử dụng**: `student` (Free) và `pro_student` (Pro)
* **URL kiểm thử**: `http://localhost:8080/quiz/result/{attemptId}`, `http://localhost:8080/quiz/{id}`
* **Danh sách Test Cases**:
  1. `E2E-PDF-01`: **Feature Gating - Chặn sinh viên Free tải đề thi PDF**:
     - Đăng nhập `student` (Free), làm xong bài thi trắc nghiệm vào trang kết quả.
     - Kiểm tra nút "Xuất PDF Bài Thi": Nút bị khóa hoặc có badge yêu cầu nâng cấp PRO, cố tình bấm thì hệ thống yêu cầu nâng cấp gói cước.
  2. `E2E-PDF-02`: **Gói Pro xuất PDF bài thi trắc nghiệm thành công**:
     - Đăng nhập `pro_student`, vào bài thi đã làm, bấm nút "Xuất PDF Bài Thi" -> Trình duyệt tải ngay file `.pdf` về máy.
  3. `E2E-PDF-03`: **Kiểm tra định dạng file PDF**:
     - Mở file PDF vừa tải:
       + Font chữ tiếng Việt `Arial.ttf` hiển thị sắc nét, **không bị lỗi ô vuông hay mất dấu tiếng Việt**.
       + Có Watermark bản quyền *"StudyHub AI - HSF302"*.
       + Có Mã QR Code định danh bài thi.
       + Bảng đáp án tổng hợp (Answer Key Grid) và phần lời giải chi tiết cho từng câu hỏi.
  4. `E2E-PDF-04`: **Gói Squad cũng được mở khóa quyền xuất PDF**:
     - Đăng nhập `squad_leader` hoặc `squad_member1`, kiểm tra tính năng xuất PDF hoạt động hoàn hảo tương đương gói Pro.
  5. `E2E-PDF-05`: **Kiểm tra giao diện Landing Page & FAQ Accordion**:
     - Mở trang chủ `http://localhost:8080/`, cuộn xuống mục FAQ -> Kiểm tra cả 5 câu hỏi đều có sẵn câu trả lời hiển thị, bấm nút "Thu gọn" và "Mở tất cả" hoạt động mượt mà.
* **Bằng chứng cần nộp**: 5 ảnh chụp màn hình (`E2E-PDF-01_nhat.png` đến `E2E-PDF-05_nhat.png`) và đính kèm 1 file PDF đã xuất.

---

### 🛡️ 6. AN - ADMIN COMMAND CENTER, QUẢN TRỊ NGƯỜI DÙNG & SYSTEM API KEY
* **Tài khoản sử dụng**: `admin` (Admin), `student` (Khách kiểm tra bảo mật)
* **URL kiểm thử**: `http://localhost:8080/admin/dashboard`, `http://localhost:8080/admin/users`, `http://localhost:8080/admin/settings`
* **Danh sách Test Cases**:
  1. `E2E-ADM-01`: **Bảo vệ route quản trị (Security Access Control)**:
     - Đăng nhập tài khoản sinh viên `student`, cố tình gõ link `http://localhost:8080/admin/dashboard` trên thanh địa chỉ -> Bị chuyển hướng hoặc hiển thị trang báo lỗi từ chối truy cập 403 Forbidden.
  2. `E2E-ADM-02`: **Admin Dashboard & Thống kê 3 gói cước**:
     - Đăng nhập tài khoản `admin` -> Tự động chuyển hướng vào `/admin/dashboard`.
     - Kiểm tra các thẻ thống kê: Tổng số người dùng, phân bố người dùng theo 3 gói (Free, Pro, Squad), tổng số môn học toàn hệ thống.
  3. `E2E-ADM-03`: **Quản lý danh sách sinh viên**:
     - Vào `/admin/users` -> Hiển thị danh sách toàn bộ 8 tài khoản demo kèm vai trò, email, gói cước và số lượt RAG trong ngày.
     - Thử tìm kiếm sinh viên theo username.
  4. `E2E-ADM-04`: **Đổi gói cước trực tiếp cho sinh viên**:
     - Tại tài khoản `student` (đang là Free), Admin bấm nút "Nâng lên Pro" -> Gói cước của `student` lập tức đổi thành PRO.
     - Thử hạ cấp lại về FREE -> Cập nhật thành công ngay lập tức.
  5. `E2E-ADM-05`: **Reset Quota RAG cho sinh viên**:
     - Một sinh viên đã chat hết 50 câu (hoặc số câu > 0), Admin bấm nút "Reset RAG" -> Số lượt chat của sinh viên đó về lại `0`, mở khóa lại cho sinh viên chat tiếp.
  6. `E2E-ADM-06`: **Quản trị System Gemini API Key & Test Ping**:
     - Vào `/admin/settings` -> Kiểm tra khóa API hệ thống được che giấu bảo mật (chỉ hiện đuôi `****`).
     - Bấm nút "Kiểm Tra Kết Nối AI (Test Ping)" -> Hệ thống gửi ping đến Google Gemini API và trả về kết quả màu xanh *"Kết nối thành công (Latency: ...ms)"*.
* **Bằng chứng cần nộp**: 6 ảnh chụp màn hình (`E2E-ADM-01_an.png` đến `E2E-ADM-06_an.png`).

---

## 📅 5. TIẾN ĐỘ THỰC HIỆN & DEADLINE

* **Bắt đầu**: 13:00 - Thứ Hai, 02/11/2026
* **Hạn chót nộp ảnh bằng chứng**: **20:00 - Thứ Hai, 02/11/2026**
* **Địa điểm nộp ảnh**: Tạo thư mục `docs/evidence/` trên nhánh của mình và đẩy lên, hoặc gửi trực tiếp ảnh vào nhóm chat chung.
* **Tổng kết & Khắc phục lỗi**: 20:30 - Thứ Hai, 02/11/2026: Leader Tùng cùng cả nhóm rà soát file `docs/issues/ISSUES-TRACKING.md` để đóng tất cả các bug còn tồn đọng.

Chúc toàn đội Nhóm 2 test mượt mà, bắt trọn bug và bảo vệ đồ án xuất sắc đạt điểm A+! 🚀
