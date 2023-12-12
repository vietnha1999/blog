---
title: "[AWS-DVA-C02-Stephane] IAM"
datePublished: Tue Dec 12 2023 15:54:02 GMT+0000 (Coordinated Universal Time)
cuid: clq2ivxjw000908jt819qeylm
slug: aws-dva-c02-stephane-iam

---

### Giới thiệu

* Global Service
    
* Root account được tạo mặc định, không nên được sử dụng hay chia sẻ
    
* Users: những người cùng 1 tổ chức của bạn, và có thể được nhóm lại
    
* Groups: chỉ bao gồm users, không có groups khác
    
    * Users có thể thuộc nhiều groups
        

### Permissions

* Users và groups có thể được gán cho JSON documents gọi là policies
    
    * Principal
        
        * IAM permissions policy: không được cho phép
            
        * resource-based policy: bắt buộc
            
    * Resource
        
        * IAM permission policy: bắt buộc
            
        * resource-based policy: tùy chọn, mặc định là resource attached
            
    * Condition
        
        * Statement chỉ có hiệu lực khi condition = true
            
* Áp dụng OR logic khi có nhiều statements/policies
    
* Ví dụ:
    

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ThirdStatement",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::account-id:root"
        ]
      },
      "Action": [
        "s3:List*",
        "s3:Get*"
      ],
      "Resource": [
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ],
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```

![
JSON policy document structure
](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/AccessPolicyLanguage_General_Policy_Structure.diagram.png align="left")

### Policies

* Id của policy (tuỳ chọn)
    
* Sid của statement (tùy chọn)
    
* Principal: account/user/role mà policy được áp dụng
    

### MFA

* MFA = password + security device
    
* MFA có trong AWS:
    
    * Virtual devices: Google Authenticator
        
    * Physical devices
        

### IAM Security tools

* IAM Credential report (account-level)
    
    * Danh sách users của account
        
    * Trạng thái đăng nhập của users
        
* IAM Access advisor (user-level)
    
    * Các service permissions được gán cho user và thời gian gian truy cập gần nhất của service đó
        

### IAM Roles

* Một vài services sẽ cần thực hiện hành động thay mặt bạn
    
* Để làm điều đó, cần gán permissions tới AWS service với IAM Roles