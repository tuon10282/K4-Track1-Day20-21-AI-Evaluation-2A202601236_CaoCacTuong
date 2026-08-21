# AI Support Log — Cao Các Tường

- **Học viên:** Cao Các Tường
- **Mã học viên (MHV):** 2A202601236
- **Khóa học:** Track 1 — AI Evaluation Lab (Day 20–21)
- **Vai trò trong nhóm:** Thiết kế User Input Grid, xây dựng làn Code Check mở rộng, thực hiện gán nhãn độc lập (Human Annotator), phân tích Calibration Loop và tổng hợp Scorecard / Gate Decision.

---

## 1. Ba Câu hỏi Tự soi về Việc sử dụng AI

### 🔹 1. AI đã giúp tôi ở đâu?
- **Paraphrase và sinh biến thể câu hỏi:** Sau khi nhóm đã khóa cứng 4 dimensions và các tổ hợp trong User Input Grid, tôi dùng AI để sinh các cách diễn đạt tự nhiên cho người dùng (ví dụ: các câu hỏi deixis chỉ trỏ ngắn gọn, câu hỏi out-of-scope ngẫu nhiên).
- **Brainstorm các assertion cho làn Code Check:** AI gợi ý cấu trúc Python logic để kiểm tra tính hợp lệ của trường `scope` (`check_scope_values`) và kiểm tra tính rỗng của `sources` khi từ chối (`check_refusal_sources`).
- **Phân tích pattern từ Confusion Matrix:** AI hỗ trợ gom nhóm và làm nổi bật 2 pattern sai số chính của Judge v1:
  - *Pattern 1 (Overly strict):* False Negative ở `sc-03` do chỉ nhìn vào trường `quote` bị rút gọn.
  - *Pattern 2 (Overly lenient):* False Positive ở `sc-20` do bỏ qua lỗi sư phạm trên câu hỏi cụt 1 từ `"Hỏi?"`.
- **Soạn nháp cấu trúc báo cáo PM:** Hỗ trợ định dạng bảng biểu và khung sườn 5 phần trong `deliverables/REPORT.md` để trình bày rõ ràng, mạch lạc theo chuẩn Product Management.

---

### 🔹 2. AI sai, hời hợt hoặc làm mất coverage ở đâu?
- **Ảo giác về ý định người dùng (False Positive ở `sc-20`):** LLM Judge `gpt-4o-mini` ban đầu chấm `PASS` và cho điểm tuyệt đối 1.0 cho câu hỏi cụt `"Hỏi?"` chỉ vì thấy nội dung Tutor xuất ra có nguồn dẫn đúng trong slide s01 và s65. AI hoàn toàn bỏ qua việc người dùng chưa đặt câu hỏi cụ thể và hành vi sư phạm đúng phải là hỏi lại (`ask for clarification`).
- **Khắt khe máy móc về độ dài trích dẫn (False Negative ở `sc-03`):** Judge bắt lỗi `sc-03` vì cho rằng Tutor "tự ý bịa chi tiết phương pháp" chỉ vì trường `quote` trong JSON bị tóm tắt 1 câu, trong khi toàn bộ nội dung kỹ thuật đó đều nằm sẵn trong section `types-of-graders-for-agents` của corpus.
- **Thiếu chiều sâu khi sinh test case:** Khi được yêu cầu sinh câu hỏi thử nghiệm, AI có xu hướng chỉ sinh các câu hỏi lý thuyết rõ ràng, dễ trả lời, bỏ qua các ô rủi ro cao như câu hỏi deixis chỉ trỏ, bẫy trích dẫn section ảo không tồn tại, hoặc nỗ lực prompt injection.

---

### 🔹 3. Tôi đã tự sửa hoặc quyết định lại điều gì?
- **Trực tiếp khóa Dimensions và Lưới Coverage:** Tôi và nhóm tự tay thiết kế 4 dimensions và chọn đúng 20 scenarios đại diện, kiên quyết loại bỏ các câu trùng lặp để bảo đảm độ bao phủ thực tế.
- **Tự gán nhãn tay 100% (Human Golden Ground Truth):** Tự mình đọc từng câu trả lời trong `results-v1.jsonl` để chấm độc lập (`deliverables/evidence/labels-tuong.csv`), không dùng AI để gán nhãn thay thế. Trực tiếp tranh luận và thẩm định đối chiếu với corpus tài liệu để giải quyết 4 ca bất đồng.
- **Tự hiệu chuẩn Judge Prompt v2:** Bổ sung 2 quy tắc cốt lõi (Quy tắc Quote rút gọn và Quy tắc câu hỏi rỗng/cụt) cùng 2 ví dụ Near-Miss thực tế từ bài học vào `judge-prompt-v2.md` để nâng độ chuẩn hóa của Judge lên 100%.
- **Tự chốt Ngưỡng Chất lượng & Phán quyết Gate:** Nhóm tự thiết lập các ngưỡng Hard Gate / Soft Gate trước khi xem số và đưa ra phán quyết cuối cùng **SHIP WITH CONDITIONS** với điều kiện sửa System Prompt của Tutor.

---

## 2. Bảng Nhật ký Sử dụng AI (AI Support Log Table)

| # | Bước thực hiện | AI dùng để làm gì | Cách tôi kiểm chứng & Quyết định lại |
|:---:|---|---|---|
| **1** | **Phase 1: Input Grid** | Gợi ý các cách hỏi tự nhiên cho từng ô trong lưới. | Tự lọc bỏ các câu trùng lặp, chọn lọc 20 scenarios phủ đủ 4 nhóm người dùng và các ô rủi ro cao. |
| **2** | **Phase 2: Human Labeling** | *Không sử dụng AI* (Tuân thủ nghiêm ngặt quy tắc bài thi). | Tự tay đọc từng trace và gán nhãn độc lập vào file `labels-tuong.csv`. |
| **3** | **Phase 3: Code Checks** | Gợi ý cú pháp Python cho các assertion logic. | Tự tích hợp 2 hàm `check_scope_values` và `check_refusal_sources` vào `code_checks.py` và chạy kiểm thử offline 43/43 tests pass. |
| **4** | **Phase 4: Calibration** | Tóm tắt nguyên nhân gây lệch điểm giữa Judge và Human. | Bác bỏ phán quyết sai của Judge ở `sc-03` và `sc-20`; tự viết lại rubric và thêm Near-Misses vào `judge-prompt-v2.md`. |
| **5** | **Phase 5: Scorecard** | Định dạng bảng biểu Pass rate và tính toán chỉ số TPR/TNR. | Tự đối chiếu số liệu với file data thô trong `evidence/`, phân rã 7 lát cắt (Slice breakdown) để tránh che giấu regression. |
| **6** | **Phase 6: Verdict & Report**| Soạn nháp ngôn ngữ báo cáo PM theo 5 phần. | Tự đưa ra phán quyết **SHIP WITH CONDITIONS**, xác lập kế hoạch Production Monitoring (Sample 10%, Alert khi Groundedness < 90%). |
