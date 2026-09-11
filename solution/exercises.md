# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *Khi temperature tăng, nội dung cốt lõi vẫn nói về hang Sơn Đoòng nhưng cách diễn đạt trở nên đa dạng, giàu cảm xúc và nhiều chi tiết hơn. Ở temperature 0.0–0.5, phản hồi khá ổn định và có cấu trúc rõ ràng; ở 1.0–1.5, model sáng tạo hơn nhưng cũng dễ thêm các thông tin chưa chắc chính xác, vì vậy cần kiểm chứng kỹ.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ đặt temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức thấp giúp phản hồi nhất quán, chính xác và bám sát chính sách, đồng thời vẫn đủ linh hoạt để câu trả lời không quá máy móc.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Với 10,5 triệu token đầu ra mỗi ngày, chi phí ước tính của GPT-4o là 105 USD/ngày, còn GPT-4o-mini là 6,30 USD/ngày; như vậy GPT-4o đắt hơn khoảng 16,67 lần nếu chỉ xét chi phí output theo bảng giá của lab. GPT-4o xứng đáng cho các tác vụ phức tạp cần suy luận và độ chính xác cao, chẳng hạn phân tích hợp đồng; GPT-4o-mini phù hợp hơn cho các câu hỏi hỗ trợ khách hàng phổ biến, phân loại yêu cầu hoặc trả lời FAQ.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Với persona giáo viên tiểu học, model sử dụng từ ngữ đơn giản, biểu tượng cảm xúc và ví dụ “cuốn nhật ký lớp học” để trẻ dễ hình dung. Với persona chuyên gia tài chính, phản hồi dài và chuyên sâu hơn, sử dụng các thuật ngữ như DLT, hash, tính bất biến, phi tập trung, Proof of Work và Proof of Stake. System prompt đã thay đổi rõ rệt giọng điệu, mức độ chi tiết, từ vựng và cách tổ chức câu trả lời dù câu hỏi người dùng không đổi. Phản hồi chuyên gia còn bị ngắt do đạt giới hạn output, cho thấy persona chuyên sâu có thể cần max_tokens lớn hơn.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Đoạn văn có 110 từ. Cách ước lượng số từ / 0,75 cho kết quả khoảng 146,67 token, trong khi count_tokens đếm được 124 token; hai kết quả chênh lệch khoảng 15,45%, trong đó cách đếm từ đã ước lượng cao hơn. Sự khác biệt xuất hiện vì tokenizer chia văn bản thành các đơn vị subword chứ không theo ranh giới từ; dấu tiếng Việt, từ ghép và mức độ xuất hiện của tiếng Việt trong dữ liệu huấn luyện cũng ảnh hưởng cách tách token. Vì vậy, tiếng Việt có thể cần nhiều token hơn tiếng Anh có nội dung hoặc độ dài tương đương, tỷ lệ cụ thể phụ thuộc tokenizer và đoạn văn.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng nhất khi model tạo phản hồi dài hoặc có độ trễ cao, chẳng hạn chatbot, trợ lý viết nội dung và sinh mã, vì người dùng có thể đọc phần đầu ngay mà không phải chờ toàn bộ kết quả. Nó cải thiện cảm giác tốc độ nhưng làm việc xử lý lỗi và ghép nội dung phức tạp hơn. Non-streaming phù hợp hơn khi phản hồi ngắn hoặc chương trình cần toàn bộ kết quả trước khi tiếp tục, ví dụ phân loại, trích xuất JSON, kiểm tra định dạng hoặc xử lý theo batch.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff tăng dần thời gian chờ giữa các lần thử, nhờ đó giảm áp lực lên API đang quá tải và tạo thêm thời gian để dịch vụ phục hồi. Với delay cố định, hàng nghìn client có thể cùng gửi lại request sau đúng một khoảng thời gian, tạo thành “retry storm” và khiến server tiếp tục quá tải. Trong hệ thống thực tế, nên kết hợp exponential backoff với jitter ngẫu nhiên để các client không retry đồng thời.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Tôi chọn persona: “Bạn là trợ giảng AI thân thiện, trả lời chính xác, ngắn gọn và luôn dùng tiếng Việt.” Cụm “trả lời chính xác, ngắn gọn” giúp phản hồi tập trung, dễ đọc và hạn chế tiêu tốn token không cần thiết. Yêu cầu “luôn dùng tiếng Việt” giữ ngôn ngữ nhất quán, phù hợp với người học trong khóa.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất của trợ lý là chỉ giữ ba lượt hội thoại gần nhất, nên có thể quên thông tin quan trọng được đề cập từ đầu phiên. Tôi sẽ cải thiện bằng cách tóm tắt các lượt cũ trước khi loại chúng khỏi history và lưu bản tóm tắt như một message ngữ cảnh. Khi tạo request mới, chương trình sẽ gửi persona, bản tóm tắt dài hạn và ba lượt gần nhất để giữ được thông tin quan trọng mà không làm số token tăng mãi.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
