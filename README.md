# 🌐 Challenge 8 - Nhóm 208 - Hướng dẫn chi tiết triển khai hạ tầng

**CHALLENGE 8: CI/CD PIPELINE FOR AUTOMATING WINDOWS SECURITY PATCHING FROM VULNERABILITY REPORT**

![example](a.png)
---
## 📑 Table of Contents

- [Prerequisites](#-prerequisites)


- [Hướng dẫn cài đặt Terraform bằng Chocolatey trên Windows](#hướng-dẫn-cài-đặt-terraform-bằng-chocolatey-trên-windows)
  - [Bước 1: Cài đặt Chocolatey](#bước-1-cài-đặt-chocolatey)
  - [Bước 2: Cài đặt Terraform](#bước-2-cài-đặt-terraform)

  
- [Kết nối Terraform với AWS](#kết-nối-terraform-với-aws)
  - [Bước 1: Cài đặt AWS CLI](#bước-1-cài-đặt-aws-cli)
  - [Bước 2: Tạo người dùng IAM và lấy thông tin truy cập](#bước-2-tạo-người-dùng-iam-và-lấy-thông-tin-truy-cập)
  - [Bước 3: Cấu hình AWS CLI](#bước-3-cấu-hình-aws-cli)
  - [Bước 4: Kiểm tra kết nối](#bước-4-kiểm-tra-kết-nối)


- [Cài đặt mã nguồn và quản lý GitHub Repository](#cài-đặt-các-gói-mã-nguồn)  
  - [Bước 1: Các kho lưu trữ](#bước-1-các-kho-lưu-trữ)  
  - [Bước 2: Hướng dẫn tải và giải nén](#bước-2-hướng-dẫn-tải-và-giải-nén) 
 
- [Chỉnh sửa tham số](#chỉnh-sửa-tham-số)  
  - [Bước 1: Sửa code Terraform](#1-chỉnh-sửa-code-terraform-để-chạy-demo)  
  - [Bước 2: Sửa code vulnerability-scripts](#2-chỉnh-sửa-code-vulnerability-scripts-để-chạy-demo)



- [Hướng dẫn tạo repository GitHub cho `vulnerability-scripts`](#hướng-dẫn-tạo-repository-github-cho-vulnerability-scripts)
  - [Bước 1: Tạo repository mới trên GitHub](#bước-1-tạo-repository-mới-trên-github)
  - [Bước 2: Kết nối repository từ máy tính](#bước-2-kết-nối-repository-từ-máy-tính)


  


- [Thiết lập GitHub Actions với AWS Credentials](#thiết-lập-github-actions-với-aws-credentials)  
  - [Bước 1. Tạo Access Key](#1-tạo-access-key)  
  - [Bước 2. Thêm Secrets vào GitHub](#2-thêm-secrets-vào-github)  
 
- [Thực hiện chạy code Terraform demo](#thực-hiện-chạy-code-terraform-cho-demo)  
  
---

## Prerequisites

Trước khi bắt đầu, bạn cần cài đặt:
- Bạn phải có [Tài khoản AWS](https://aws.amazon.com/vi/free/?trk=947f595b-f07f-42a1-bfc4-acf832730bac&sc_channel=ps&ef_id=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE:G:s&s_kwcid=AL!4422!3!566333972302!e!!g!!t%E1%BA%A1o%20t%C3%A0i%20kho%E1%BA%A3n%20aws!15461586425!133325773849&gad_campaignid=15461586425&gbraid=0AAAAADjHtp_VsxSh0NtOGy0Q984Eg9pDc&gclid=CjwKCAjw7MLDBhAuEiwAIeXGIVdjnkAmtIdxP6A3pQo_RD5aR_WbnyoGnObQJq8dK6ZkvrULgqdhnhoCkT4QAvD_BwE&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
- Cài đặt [Terraform](https://developer.hashicorp.com/terraform/downloads)
- Có tài khoản GitHub ([https://github.com](https://github.com))
- Đã cài đặt **Git** trên máy ([Tải Git](https://git-scm.com/))
---
# Hướng dẫn cài đặt Terraform bằng Chocolatey trên Windows

Chocolatey là một trình quản lý gói (Package Manager) dành cho hệ điều hành Windows cho phép bạn cài đặt, cập nhật và gỡ bỏ phần mềm dễ dàng thông qua CLI (Command Line Interface).

### Bước 1: Cài đặt Chocolatey

1. Mở Windows Powershell trên máy với quyền Admin (Run as administrator)
2. Chạy câu lệnh sau:
```bash

Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = `
[System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

```
3. Kiểm tra phiên bản của Chocolatey bằng câu lệnh:
```bash

choco -v

```
4. Kết quả: 
```bash

2.3.0

```
   
### Bước 2: Cài đặt Terraform

1. Trên Windows Powershell, chạy câu lệnh sau:

```bash

choco install terraform -y

```
2. Kiểm tra phiên bản của Terraform bằng câu lệnh:

```bash

terraform -v

```
3. Kết quả:

```bash

Terraform v1.12.2
on windows_amd64

```

- Bạn có thể xem hướng dẫn cài đặt terraform cho Linux, MacOs [tại đây]( https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
---

#  Kết nối Terraform với AWS 

### Bước 1. Cài đặt AWS CLI 

#### 1.1 Cài AWS CLI bằng 
```bash
C:\> msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

#### 1.2 Để xác nhận việc cài đặt, hãy mở menu Start, tìm kiếm "cmd" để mở cửa sổ dòng lệnh (Command Prompt), và trong cửa sổ đó, sử dụng lệnh aws --version.

```bash
C:\> aws --version
aws-cli/2.19.1 Python/3.11.6 Windows/10 exe/AMD64 prompt/off
```
### Bước 2. Tạo thông tin người dùng và thông tin truy cập
#### 2.1 Vào AWS console 
Tìm kiếm [IAM](https://console.aws.amazon.com/iam)
#### 2.2 Tạo IAM User
- Chọn Users > nhấn nút Add users

- Nhập tên (ví dụ: terraform-user)

- Chọn Access key - Programmatic access

- Nhấn Next
#### 2.3 Gán quyền
- tick chọn **AdministratorAccess**
#### 2.4 Tạo Access Key
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

### Bước 3. cấu hình AWS CLI
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
# Cài đặt các gói mã nguồn

## Bước 1: Các kho lưu trữ

- **Vulnerability Scripts:** [https://github.com/imLeHuyHoang/vulnerability-scripts.git](https://github.com/imLeHuyHoang/vulnerability-scripts.git)
- **Hackathon Terraform:** [https://github.com/imLeHuyHoang/hackathonterraform.git](https://github.com/imLeHuyHoang/hackathonterraform.git)


## Bước 2. Hướng dẫn tải và giải nén

### Bước 2.1: Tải file ZIP từ GitHub

1. Truy cập link GitHub của mỗi kho.
2. Nhấn nút **Code** (màu xanh lá).
3. Chọn **Download ZIP** để tải file `.zip` về máy.



### Bước 2.2: Giải nén file ZIP

Sau khi tải về, bạn sẽ có 2 file ZIP:

- `vulnerability-scripts-main.zip`
- `hackathonterraform-main.zip`

Tiến hành giải nén:

- Nếu dùng **Windows**: Nhấp chuột phải vào file ZIP → Chọn **Extract All...**.

Sau khi giải nén, bạn phải đổi tên các folder thành `vulnerability-scripts` và `hackathonterraform`


# Chỉnh sửa tham số 

## 1. Chỉnh sửa code Terraform để chạy demo
### Bước 1: Mở thư mục dự án bằng VS Code

1. Mở **Visual Studio Code**.
2. Chọn **`File` → `Open Folder...`**.
3. Duyệt đến thư mục `hackathonterraform`
4. Bấm **`Open`**.

VS Code sẽ mở toàn bộ project, hiển thị cấu trúc file bên thanh bên trái.

### Bước 2.2: Chỉnh sửa code Terraform

- Mở file `variables.tf` tại thư mục `root`

![example](png10.png)
- Thực hiện thay đổi biến `project_name` thành biến duy nhất của bạn.
![example](anh11.png)

- Lưu lại code

## 2. Chỉnh sửa code Vulnerability-scripts để chạy demo


### Bước 1: Mở thư mục dự án bằng VS Code

1. Mở **Visual Studio Code**.
2. Chọn **`File` → `Open Folder...`**.
3. Duyệt đến thư mục `Vulnerability-scripts `
4. Bấm **`Open`**.

VS Code sẽ mở toàn bộ project, hiển thị cấu trúc file bên thanh bên trái.

### Bước 2.2: Chỉnh sửa code Terraform

- Mở file `XXX` tại thư mục `XXX`

![example]()
- Thực hiện thay đổi biến `XXX` thành biến duy nhất của bạn.
![example]()

- Lưu lại code
# Hướng dẫn tạo repository GitHub cho `vulnerability-scripts`



### Bước 1: Tạo repository mới trên GitHub

1. **Đăng nhập** vào tài khoản GitHub.
2. Ở góc phải trên cùng, bấm nút **`+`** ➜ chọn **`New repository`**.
3. Điền thông tin:
   - **Repository name:** `vulnerability-scripts`
   - **Description:** Mô tả ngắn gọn, ví dụ: *Scripts for vulnerability scanning and management*
   - Chọn **Public** hoặc **Private** tuỳ ý.
   - **Không tick** vào *Initialize this repository with a README* (vì bạn đã có code sẵn).
4. Bấm **Create repository**.

---

### Bước 2: Kết nối repository từ máy tính

#### Mở terminal/command line và chạy các lệnh sau:

```bash
# Di chuyển vào thư mục vulnerability-scripts (chỉnh lại đường dẫn cho đúng)
cd path/to/vulnerability-scripts

# Khởi tạo Git (nếu chưa có)
git init

# Thêm remote origin trỏ đến repo GitHub vừa tạo
git remote add origin https://github.com/<YOUR_USERNAME>/vulnerability-scripts.git

# Thêm toàn bộ file
git add .

# Commit lần đầu
git commit -m "Initial commit"

# Đặt nhánh chính tên 'main'
git branch -M main

# Đẩy code lên GitHub
git push -u origin main
```

# Thiết lập GitHub Actions với AWS Credentials

## 1. Tạo Access Key
Truy cập AWS Console, vào IAM, chọn User mà bạn đã configure AWS CLI, sau đó chọn **Security credentials**
![example](anh1.png)
Chọn **Create access key**
![example](anh2.png)
Chọn **(CLI)** rồi **Done**
![example](anh3.png)
Đặt **tag value** cho Github Actions key
![example](anh8.png)
Tạo thành công 
![example](anh5.png)


## 2. Thêm Secrets vào GitHub
### Vào repo `vulnerability-scripts` chọn **setting**
![example](anh6.png)

### Vào cột bên trái chọn **secrets and varialbe** -> chọn **Actions**
### Chọn **New repository secrect**
![example](anh9.png)
#### Tạo Github Action  **access key**

Điền phần **name**
```bash
AWS_ACCESS_KEY_ID
```
**Secret**
```bash
<dán key của bạn ở CLI AWS>
```
Xong bấm tạo **add secret**

#### Tạo **Secret access key**

Điền phần **name**
```bash
AWS_SECRET_ACCESS_KEY
```
**Secret**
```bash
<dán key của bạn ở CLI AWS>
```
Xong bấm tạo **add secret**
# Thực hiện chạy code Terraform cho demo
Sử dụng VScode để mở thư mục **hackathonterraform**, sau đó mở terminal lên và thực hiện các câu lệnh sau: 
- Khởi tạo Terraform:
```
Terraform init
```
- Kiểm tra trước những thay đổi Terraform sẽ thực hiện:
```
Terraform plan
```
- Áp dụng các thay đổi để tạo hoặc cập nhật hạ tầng:
```
Terraform apply
```
- Xóa toàn bộ tài nguyên được quản lý bởi Terraform (Lưu ý: chỉ xóa toàn bộ khi đã thực hiện xong demo):
```
Terraform destroy
```
# Đẩy file data lên Amazon S3 để chạy pipeline
Yêu cầu
Đã chạy câu lệnh terraform apply
Đã có file data

Kiểm tra Bucket đã tạo
Sau khi đã chạy terraform apply thành công, bạn truy cập AWS Console, truy cập dịch vụ S3 và kiểm tra xem đã có Bucket hay chưa

Chọn **<TÊN BUCKET CỦA BẠN>**
![example](s3-1.png)



Chọn **raw-vulnerability-data/**
![example](s3-2.png)
Bấm **Upload**
![example](s3-3.png)
Bấm **Add folder**
![example](s3-4.png)
Chọn file **2025-Quarter-2.xlsx** -> click **Upload**
![example](s3-5.png)
Đây là giao diện khi **Upload** file thành công 
![example](s3-6.png)

