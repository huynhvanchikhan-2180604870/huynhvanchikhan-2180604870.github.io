# huynhvanchikhan-2180604870.github.io
# Disaster Recovery Automation cho DevOps Infrastructure

## 📌 Giới thiệu
Dự án này triển khai **giải pháp DR (Disaster Recovery) tự động** cho hạ tầng DevOps trên AWS, bao gồm:
- **Backup Automation**: Sao lưu tự động đa vùng (cross-region) cho RDS.
- **DR Automation**: Quy trình khôi phục tự động tại primary hoặc DR region.
- **Recovery Testing Automation**: Lambda + SSM kiểm thử định kỳ khả năng khôi phục.
- **Compliance Validation**: AWS Config & Conformance Pack đảm bảo tuân thủ.
- **Monitoring Setup**: CloudWatch Alarms & SNS cảnh báo.
- **Documentation & Operational Procedures**: Runbook và hướng dẫn chi tiết.
- **Cost Analysis**: Đánh giá chi phí triển khai và vận hành DR.

---

## 🚀 Kiến trúc tổng quan
- **Primary Region**: us-east-1
- **DR Region**: us-west-2
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

## 📜 Quy trình khôi phục (Runbook)

### Same-Region Restore
1. Chọn snapshot mới nhất từ Backup Vault primary.
2. Restore thành instance mới.
3. Cập nhật endpoint DB trong EC2 backend.

### Cross-Region Restore
1. Chuyển sang DR Region.
2. Restore snapshot copy sang DR subnet & SG.
3. Trỏ ứng dụng sang endpoint DR.

---

## 📊 Chi phí dự kiến
- **AWS Backup**: Chi phí backup & cross-region copy.
- **RDS**: Chi phí chạy instance khi test/restore.
- **Lambda**: Theo số lần chạy test.
- **CloudWatch/SNS**: Chi phí alarm & thông báo.
- **S3**: Chi phí lưu trữ FE & AWS Config logs.

---

## 🛠 Công cụ & Dịch vụ AWS sử dụng
- **Compute**: EC2, Lambda
- **Database**: RDS MySQL
- **Storage**: S3, ECR
- **Automation**: AWS Backup, EventBridge, SSM
- **Security & Compliance**: IAM, AWS Config, Conformance Pack
- **Monitoring**: CloudWatch, SNS

---

## 📧 Liên hệ
- **Author**: Huỳnh Văn Chí Khan
- **Email**: *(Điền email của bạn)*
- **GitHub**: *(Link repository)*

---

> **Note**: Dự án được tối ưu cho mục đích học tập, POC và demo DR trên AWS.  
> Trước khi triển khai production, cần đánh giá lại chi phí, RTO/RPO, bảo mật và compliance.

