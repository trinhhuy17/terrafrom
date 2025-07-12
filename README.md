# 🌐 Challenge 8 - Group 208

Hướng dẫn triển khai hạ tầng đơn giản theo từng bước để kết nối Terraform với tài khoản AWS . 

---

## 📦 Prerequisites

Trước khi bắt đầu, bạn cần cài đặt:
- Bạn phải có [Tài khoản AWS](https://aws.amazon.com/vi/free/?trk=947f595b-f07f-42a1-bfc4-acf832730bac&sc_channel=ps&ef_id=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE:G:s&s_kwcid=AL!4422!3!566333972302!e!!g!!t%E1%BA%A1o%20t%C3%A0i%20kho%E1%BA%A3n%20aws!15461586425!133325773849&gad_campaignid=15461586425&gbraid=0AAAAADjHtp_VsxSh0NtOGy0Q984Eg9pDc&gclid=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
- Cài đặt [Terraform](https://developer.hashicorp.com/terraform/downloads)

---
## Setup Terraform

---

##  Kết nối Terraform với AWS 

### Bước 1. Cài AWS CLI 


#### 1.1 Cài AWS CLI bằng 
```bash
C:\> msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

#### 1.2 Để xác nhận việc cài đặt, hãy mở menu Start, tìm kiếm "cmd" để mở cửa sổ dòng lệnh (Command Prompt), và trong cửa sổ đó, sử dụng lệnh aws --version.

```bash
C:\> aws --version
aws-cli/2.19.1 Python/3.11.6 Windows/10 exe/AMD64 prompt/off
```
### Bước 2. TẠO NGƯỜI DÙNG IAM VÀ LẤY THÔNG TIN TRUY CẬP
#### 2.1 Vào AWS console 
Tìm kiếm [IAM](https://console.aws.amazon.com/iam)
### 2.2 Tạo IAM User
- Chọn Users > nhấn nút Add users

- Nhập tên (ví dụ: terraform-user)

- Chọn Access key - Programmatic access

- Nhấn Next
### 2.3 Gán quyền
- tick chọn **AdministratorAccess**
### 2.4 Tạo Access Key
Vào IAM chọn **Security credentials**
![example](anh1.png)
Chọn **Create access key**
![example](anh2.png)
Chọn **(CLI)** rồi **Done**
![example](anh3.png)
Đặt tên cho access key 
![example](anh4.png)
Tạo thành công 
![example](anh5.png)

### Bước 3. CẤU HÌNH AWS CLI
Sau khi đã có **Access Key** và **Secret Key**, mở CMD:

```bash
aws configure
```
Sau đó nhập:
```bash
AWS Access Key ID [None]: <dán key của bạn>
AWS Secret Access Key [None]: <dán key của bạn>
Default region name [None]: ap-southeast-1  # Singapore 
Default output format [None]: json
```

### Bước 4: Kiểm tra kết nối 
Kiểm tra bằng:
```bash
aws s3 ls
```
**Nếu hiện danh sách bucket (hoặc trống), tức là CLI hoạt động thành công.**

---