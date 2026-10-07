# CHỌN CHIẾN LƯỢC PHÂN RÃ EPIC "HỦY CHUYẾN GHÉP"

## Phần 1 – Đề xuất 2 chiến lược phân rã

**Chiến lược A – Phân rã theo các bước của luồng nghiệp vụ (workflow steps)**
Chia Epic theo thứ tự các bước xảy ra khi một khách hủy chuyến:
- Feature A1: Khách gửi yêu cầu hủy chuyến ghép
- Feature A2: Hệ thống tính phí hủy
- Feature A3: Hệ thống tính lại giá cho các khách còn lại
- Feature A4: Hệ thống thông báo cho các bên liên quan

**Chiến lược B – Phân rã theo quy tắc nghiệp vụ / tình huống hủy (business rules)**
Chia Epic theo từng trường hợp hủy có quy tắc xử lý khác nhau:
- Feature B1: Hủy chuyến miễn phí (trước khi tài xế nhận chuyến)
- Feature B2: Hủy chuyến có phí 10.000đ (sau khi tài xế nhận chuyến, kể cả hủy sát thời điểm nhận)
- Feature B3: Tính lại giá và thông báo ngay cho các khách còn lại khi có người hủy

---

## Phần 2 – So sánh theo INVEST

| Tiêu chí | Chiến lược A (theo bước luồng) | Chiến lược B (theo quy tắc nghiệp vụ) |
|---|---|---|
| **Independent** | **(–)** Các Feature nối tiếp nhau: A2, A3, A4 đều phụ thuộc A1 và đầu ra của bước trước nên khó làm song song hoặc đảo thứ tự. | **(+)** B1 và B2 là hai trường hợp riêng, làm và phát hành độc lập; B3 chỉ cần có một lần hủy xảy ra. |
| **Small** | **(+)** Mỗi bước là một mảnh rất nhỏ, dễ làm xong trong một Sprint. | **(+)** Mỗi trường hợp là một lát cắt hẹp; **(–)** B2 hơi lớn vì gồm cả tính phí lẫn xử lý hủy sát lúc tài xế nhận, có thể cần tách tiếp. |
| **Valuable** | **(–)** Từng bước riêng lẻ không giúp khách hủy trọn vẹn (ví dụ có nút hủy nhưng chưa tính phí), nên chưa tạo giá trị khi phát hành riêng. | **(+)** Mỗi Feature chạy trọn từ đầu đến cuối cho một tình huống, phát hành xong là khách dùng và nhận giá trị ngay. |

---

## Phần 3 – Lựa chọn & đặc tả

**Chiến lược chọn: B – Phân rã theo quy tắc nghiệp vụ.**
Lý do: mỗi Feature là một lát cắt dọc có giá trị dùng được ngay và độc lập hơn, nên đội có thể phát hành sớm từng trường hợp (ví dụ hủy miễn phí trước) qua các Sprint tới; ngược lại chiến lược A chia theo bước nên phụ thuộc nhau và chưa có giá trị khi chỉ làm một phần.

**User Story (thuộc Feature B2 – Hủy chuyến có phí)**
- **As a** khách đi ghép,
- **I want** hủy chuyến sau khi tài xế đã nhận và bị tính phí hủy 10.000đ, với thời điểm tính phí dựa trên thời gian ghi nhận trên máy chủ,
- **So that** tôi biết rõ khoản phí phải trả và việc tính phí công bằng, ngay cả khi tôi hủy sát lúc tài xế nhận chuyến.

**Acceptance Criteria – Hủy đúng lúc tài xế vừa nhận chuyến**
- **Given** khách A đã đặt chuyến ghép, máy chủ ghi nhận tài xế nhận chuyến lúc 10:00:05 và ghi nhận yêu cầu hủy của khách A lúc 10:00:07,
- **When** hệ thống xác định có tính phí hay không bằng cách so sánh hai mốc thời gian ghi nhận trên máy chủ (không dùng giờ trên máy khách),
- **Then** vì yêu cầu hủy được ghi nhận sau thời điểm tài xế nhận chuyến, khách A bị tính phí hủy 10.000đ và được thông báo khoản phí; giá của các khách còn lại được tính lại và thông báo ngay.

*(Ngược lại, nếu máy chủ ghi nhận yêu cầu hủy trước thời điểm tài xế nhận chuyến thì khách A được hủy miễn phí.)*
