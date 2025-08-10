# 🚀 Disaster Recovery Automation cho DevOps Infrastructure

## 📌 Giới thiệu

Dự án này triển khai **giải pháp DR (Disaster Recovery) tự động** cho hạ tầng DevOps trên AWS, bao gồm:

- 🔄 **Backup Automation**: Sao lưu tự động đa vùng (cross-region) cho RDS.
- ⚡ **DR Automation**: Quy trình khôi phục tự động tại primary hoặc DR region.
- 🛠 **Recovery Testing Automation**: Lambda + SSM kiểm thử định kỳ khả năng khôi phục.
- ✅ **Compliance Validation**: AWS Config & Conformance Pack đảm bảo tuân thủ.
- 📊 **Monitoring Setup**: CloudWatch Alarms & SNS cảnh báo.
- 📖 **Documentation & Operational Procedures**: Runbook và hướng dẫn chi tiết.
- 💰 **Cost Analysis**: Đánh giá chi phí triển khai và vận hành DR.

---

## 🏗 Kiến trúc tổng quan

- **Primary Region**: `us-east-1`
- **DR Region**: `us-west-2`
- **Backend**: Java Spring Boot + MySQL RDS
- **Frontend**: ReactJS trên S3
- **Hạ tầng AWS**: VPC, EC2, RDS, S3, ECR, AWS Backup, Lambda, SSM, SNS, AWS Config.

---

## 📂 Nội dung chính

### 1. Chuẩn bị hạ tầng

- Tạo **VPC**, **Subnets**, **Security Groups**.
- Tạo **IAM Roles** cho EC2, Lambda.
- Tạo **ECR** để lưu trữ Docker images.

### 2. Triển khai Backend & Frontend

- **Backend**: Build & push Docker image → Deploy lên EC2 Runtime.
- **Frontend**: Deploy ReactJS lên S3 bucket với public access.

### 3. Backup Automation

- AWS Backup Plan hàng ngày cho RDS.
- Cross-region copy sang DR vault.

### 4. DR Automation

- Khôi phục DB từ snapshot (same-region hoặc cross-region).
- Gán Security Group/Subnet phù hợp.
- Cập nhật endpoint DB cho Backend.

### 5. Recovery Testing Automation

- Lambda function khôi phục DB từ snapshot mới nhất.
- EC2 + SSM chạy script kiểm tra dữ liệu.
- Gửi kết quả qua SNS Email.

### 6. Compliance Validation

- AWS Config với các rule RDS:
  - `rds-backup-enabled`
  - `rds-instance-deletion-protection-enabled`
  - `rds-instance-public-access-check`
  - `rds-storage-encrypted`
  - `rds-snapshots-public-prohibited`
- Deploy **Operational Best Practices for Amazon RDS** Conformance Pack.

### 7. Monitoring

- CloudWatch alarms cho RDS (CPU, Storage).
- Alarm cho Lambda DR test failures.

---

## 💰 Phân tích chi phí (Cost Analysis)

### 📊 Chi phí ước tính hàng tháng (Demo)

| Dịch vụ                  | Mô tả                                      | Ước tính (USD/tháng) | Ghi chú                    |
| ------------------------ | ------------------------------------------ | -------------------: | -------------------------- |
| Amazon RDS (db.t3.micro) | DB primary                                 |                  ~15 | Free tier nếu đủ điều kiện |
| AWS Backup               | Daily backup + copy cross-region (30 ngày) |                10–25 | Theo GB backup             |
| Cross-Region Transfer    | Copy snapshot us-east-1 → us-west-2        |              0.02/GB | 20GB ≈ $0.40/lần           |
| AWS Lambda               | DR test 1–2 lần/tháng (≤15 phút)           |                   <1 | Hầu như miễn phí           |
| Amazon S3                | FE + Config logs                           |                 ~0.5 | < 1 GB                     |
| Amazon ECR               | Docker image (~200MB)                      |                 ~0.1 | 500MB free                 |
| Amazon EC2               | BE runtime + builder (t3.micro)            |                 8–10 | Free tier nếu còn          |
| CloudWatch               | Metric + alarms                            |                  1–3 | Tuỳ retention              |
| SNS                      | Email cảnh báo                             |                 ~0.1 | 1K email free              |
| AWS Config               | Rule đánh giá compliance                   |                  3–5 | 0.003 USD/resource/giờ     |

**💡 Tổng (demo/học tập):** ~**$40–60/tháng**  
**📈 Production:** Phụ thuộc dung lượng DB, retention, tần suất test — có thể **>$100/tháng**.

### 🔍 Các yếu tố ảnh hưởng đến chi phí

- Dung lượng RDS snapshot & tần suất sao lưu/copy.
- Thời gian lưu (retention).
- Số lần DR test/tháng và kích thước instance khi restore.
- Retention CloudWatch logs.

### 💡 Chiến lược tối ưu chi phí

- Dùng **lifecycle** cho backup (dev/test: 7–15 ngày).
- Giảm tần suất DR test ở non-prod (1–2 lần/tháng).
- Restore test với instance nhỏ (ví dụ `db.t3.micro`).
- Tự xoá DB test sau khi kiểm thử (`AUTO_DELETE=true`).
- Giảm retention log (7–14 ngày) cho môi trường test.

---

## 🛠 Quy trình khôi phục (Runbook)

### Same-Region Restore

1. Chọn snapshot mới nhất từ Backup Vault primary.
2. Restore thành instance mới.
3. Cập nhật endpoint DB trong EC2 backend.

### Cross-Region Restore

1. Chuyển sang DR Region.
2. Restore snapshot copy sang DR subnet & SG.
3. Trỏ ứng dụng sang endpoint DR.

---

## 📜 Tác giả & Liên hệ

- 👤 **Author**: Huỳnh Văn Chí Khan
- 📧 **Email**: [huynhkhan91@gmail.com](mailto:huynhkhan91@gmail.com)
- 🌐 **GitHub**: [github.com/huynhvanchikhan-2180604870](https://github.com/huynhvanchikhan-2180604870)

---

> ⚠ **Lưu ý**: Dự án được tối ưu cho mục đích học tập, POC và demo DR trên AWS.  
> Trước khi triển khai production, cần đánh giá lại chi phí, RTO/RPO, bảo mật và compliance.
