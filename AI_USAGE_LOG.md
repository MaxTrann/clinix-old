# Nhật ký sử dụng AI (AI Usage Log)

Minh chứng cho tiêu chí **TC2.3** của rubric. Điền **ngay khi dùng**, không viết bù cuối kỳ — hội đồng
đối chiếu nhật ký này với lịch sử commit và với phần vấn đáp.

Quy ước:
- Mỗi lần dùng AI có ảnh hưởng đến repo (mã, test, cấu hình, tài liệu) = một dòng ở bảng 1.
- Mỗi lỗi/ảo giác của AI mà nhóm tự phát hiện = một dòng ở bảng 2, kèm commit sửa. Mục tiêu rubric Mức 5: **≥ 5 lỗi**.
- Cột "Commit sửa" ở bảng 2 ghi PR/commit đã sửa lỗi; mô tả PR nêu mã `AI-BUG-nnn` tương ứng.

## 1. Nhật ký sử dụng

| # | Ngày | Người dùng | Công cụ / model | Phạm vi (module/file) | Prompt chính | Phần AI sinh | Phần tự sửa/viết | Cách kiểm chứng | Commit/PR |
|---|------|-----------|-----------------|-----------------------|--------------|--------------|------------------|-----------------|-----------|
| 1 | 09/10/2026 | MaxTrann | Claude Code (Claude Sonnet 5.5) | Khung repo: `docker-compose*.yml`, `apps/*/Dockerfile`, `.github/workflows/ci.yml` + `cd.yml`, husky/commitlint, gitleaks, template PR/issue, `docs/`, README, CONTRIBUTING | Dựng khung repo GitHub theo stack trong SOW (React, NestJS, MySQL, Docker) và theo rubric TC2.3–2.6: CI nhiều chặng, CD lên staging, quy ước commit | Toàn bộ nội dung các file khung ở cột Phạm vi | Tạo repo, đặt quy ước, cấu hình ruleset, tự đọc và tự commit/push | `docker compose config`; chạy thử CI trên GitHub (phát hiện 2 lỗi, xem bảng 2) | `abbfc58` |
| 2 | 09/10/2026 | MaxTrann | Claude Code (Claude Sonnet 5.5) | `.github/workflows/ci.yml`, `cd.yml` | Sửa workflow bị GitHub từ chối (run 0 job) và sửa job `security` đỏ | Job `detect`, các điều kiện `needs.detect.outputs.*`, đổi version `trivy-action` | Tìm nguyên nhân qua tab Actions, review, tự commit/push, mở PR | `actionlint` (đã thử trên mẫu sai để chắc nó bắt được lỗi cũ); CI trên PR xanh | PR #6 |

## 2. Lỗi / ảo giác của AI đã phát hiện

| Mã | Ngày | Mô tả lỗi | Nguyên nhân (phân tích) | Cách phát hiện | Commit sửa |
|----|------|-----------|--------------------------|----------------|------------|
| AI-BUG-001 | 09/10/2026 | Dùng `hashFiles()` ở `if` cấp job trong `ci.yml` và `cd.yml`; GitHub từ chối workflow, run hiện 0 job | AI không nhớ ràng buộc context availability của GitHub Actions (hàm này chỉ dùng được ở cấp step); lúc kiểm chỉ chạy kiểm cú pháp YAML nên không bắt được | Tab Actions: run đỏ, 0 job, tên run là đường dẫn file; đối chiếu bằng API run/jobs và tài liệu | PR #6 |
| AI-BUG-002 | 09/10/2026 | Ghi `aquasecurity/trivy-action@0.28.0` — tag không tồn tại, job `security` đỏ ở bước "Set up job" | AI viết version theo trí nhớ, không đối chiếu danh sách release thật (repo đã đổi sang tag có tiền tố `v`) | Annotation của check-run: "unable to find version `0.28.0`"; tra danh sách tag trên GitHub | PR #6 |

## 3. Quy trình kiểm soát đầu ra AI

<!-- Mức 5 yêu cầu quy trình thành hệ thống. Điền khi nhóm chốt, ví dụ: -->
- Review bằng checklist ở `.github/PULL_REQUEST_TEMPLATE.md` cho mọi PR có AI.
- Đối chiếu API/thư viện với tài liệu chính thức trước khi chấp nhận.
- Kiểm tra giấy phép khi AI đề xuất thư viện hoặc đoạn mã dài.
- Không đưa dữ liệu bệnh nhân thật, secret, khóa API vào prompt.
