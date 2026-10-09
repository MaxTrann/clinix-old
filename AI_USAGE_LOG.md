# Nhật ký sử dụng AI (AI Usage Log)

Minh chứng cho tiêu chí **TC2.3** của rubric. Điền **ngay khi dùng**, không viết bù cuối kỳ — hội đồng
đối chiếu nhật ký này với lịch sử commit và với phần vấn đáp.

Quy ước:
- Mỗi lần dùng AI có ảnh hưởng đến repo (mã, test, cấu hình, tài liệu) = một dòng ở bảng 1.
- Mỗi lỗi/ảo giác của AI mà nhóm tự phát hiện = một dòng ở bảng 2, kèm commit sửa. Mục tiêu rubric Mức 5: **≥ 5 lỗi**.
- Commit sửa lỗi AI ghi mã ở cuối tiêu đề: `fix(booking): sửa truy vấn khung giờ [AI-BUG-003]`.

## 1. Nhật ký sử dụng

| # | Ngày | Người dùng | Công cụ / model | Phạm vi (module/file) | Prompt chính | Phần AI sinh | Phần tự sửa/viết | Cách kiểm chứng | Commit/PR |
|---|------|-----------|-----------------|-----------------------|--------------|--------------|------------------|-----------------|-----------|
| 1 |      |           |                 |                       |              |              |                  |                 |           |

## 2. Lỗi / ảo giác của AI đã phát hiện

| Mã | Ngày | Mô tả lỗi | Nguyên nhân (phân tích) | Cách phát hiện | Commit sửa |
|----|------|-----------|--------------------------|----------------|------------|
| AI-BUG-001 |  |  |  |  |  |

## 3. Quy trình kiểm soát đầu ra AI

<!-- Mức 5 yêu cầu quy trình thành hệ thống. Điền khi nhóm chốt, ví dụ: -->
- Review bằng checklist ở `.github/PULL_REQUEST_TEMPLATE.md` cho mọi PR có AI.
- Đối chiếu API/thư viện với tài liệu chính thức trước khi chấp nhận.
- Kiểm tra giấy phép khi AI đề xuất thư viện hoặc đoạn mã dài.
- Không đưa dữ liệu bệnh nhân thật, secret, khóa API vào prompt.
