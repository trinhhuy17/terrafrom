# 🌐 Challenge 8 - Group 208

Hướng dẫn triển khai hạ tầng đơn giản theo từng bước để kết nối Terraform với tài khoản AWS . 

---

## 📦 Prerequisites

Trước khi bắt đầu, bạn cần cài đặt:
- Bạn phải có [Tài khoản AWS](https://aws.amazon.com/vi/free/?trk=947f595b-f07f-42a1-bfc4-acf832730bac&sc_channel=ps&ef_id=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE:G:s&s_kwcid=AL!4422!3!566333972302!e!!g!!t%E1%BA%A1o%20t%C3%A0i%20kho%E1%BA%A3n%20aws!15461586425!133325773849&gad_campaignid=15461586425&gbraid=0AAAAADjHtp_VsxSh0NtOGy0Q984Eg9pDc&gclid=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
- Cài đặt [Terraform](https://developer.hashicorp.com/terraform/downloads)

---
## Hướng dẫn cài đặt Terraform

### Bước 1: Cài đặt Terraform cho Windows
Truy cập trang cài đặt Terraform qua đường link sau: https://developer.hashicorp.com/terraform/install

1. Chọn cấu hình phù hợp với máy của bạn:

<img width="972" height="277" alt="image" src="https://github.com/user-attachments/assets/36aef5f8-a8c0-476f-b68c-920af10565dc" />

2. Sau khi cài đặt thành công, thực hiện giải nén file .zip, bên trong bao gồm 1 file terraform.exe và 1 file license.txt

<img width="1286" height="352" alt="Extract" src="https://github.com/user-attachments/assets/3b19c9ea-0726-4d02-a3cf-39e15fadbcda" />

### Bước 2: Cài đặt Enviroment variable

1. Thực hiện copy đường dẫn tới file:

<img width="1283" height="293" alt="Copy_path" src="https://github.com/user-attachments/assets/d242a2e5-3100-4c4e-b05f-2b0f6ad9d8f4" />

2. Tìm và chọn "Edit the system enviroment variables" trên thanh search:

<img width="1005" height="907" alt="Edit_variables_1" src="https://github.com/user-attachments/assets/5e9c5ef1-55c5-4609-8388-8c53007bf146" />

3. Chọn Enviroment Variables..

<img width="542" height="566" alt="Edit_variables_2" src="https://github.com/user-attachments/assets/6a6921f4-d919-4d3b-936b-4ad4977e43fc" />

4. Click vào dòng chứa Path trong "System variables" và chọn Edit
  
<img width="705" height="771" alt="Edit_variables_3" src="https://github.com/user-attachments/assets/9194562f-d92b-4319-ac02-2e63fa707b44" />

5. Chọn New và paste đường dẫn tới Folder terraform bạn vừa tải về sau đó chọn Ok

<img width="592" height="655" alt="Edit_variables_4" src="https://github.com/user-attachments/assets/e94af67c-99e2-462d-b446-0bfcacaf9cc2" />

### Bước 3: Kiểm tra cài đặt:

1. Gõ cmd trên thanh tìm kiếm và sau đó thực hiện câu lệnh

```bash

terraform -v

```
2. Nếu như màn hình trả ra kết quả như bên dưới thì bạn đã cài đặt thành công, câu lệnh hiển thị phiên bản của terraform trên máy của bạn.

<img width="676" height="150" alt="terraform_version" src="https://github.com/user-attachments/assets/3b609edc-bdc1-45fa-9bdb-f4c5090ac62a" />

- Bạn có thể xem hướng dẫn cài đặt terraform cho Linux, MacOs tại đây: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
---

##  Kết nối Terraform với AWS 

### Bước 1. Cài CLI 


#### 1.1 Cài CLI bằng 
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
