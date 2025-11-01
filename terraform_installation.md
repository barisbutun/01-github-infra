# Terraform Windows Kurulum Adımları
---
### Adım 1
 https://developer.hashicorp.com/terraform/downloads sayfasına git
 
 ### Adım 2
 Windows 64-bit ZIP dosyasını indirin.

 ### Adım 3
 ZIP dosyasını çıkarın ve içindeki terraform.exe dosyasını şu klasöre taşıyın:
   ```makefile
   C:\Program Files\Terraform
   ```
### Adım 4
Ortam Değişkenlerine terraformun PATH'ini ekle
   Denetim Masası → Sistem → Gelişmiş Sistem Ayarları → Ortam Değişkenleri → Path → Düzenle → Yeni → C:\Program Files\Terraform
   
### Adım 5
CMD veya PowerShell’i aç:
   ```cmd
   terraform -version
   ```
   Çıktı sürüm bilgisi veriyorsa kurulum tamamdır.

### Adım 6
Çıktı sürüm bilgisi vermiyorsa Powerhell ya da CMD ekranını yönetici olarak çalıştırıp aşağıdaki kodu kopyala
   ```cmd
   setx PATH "%PATH%;C:\Program Files\Terraform"

   ```
   Kodu yazdıktan sonra ekranı kapat

### Adım 7
Tekrar cmd ekranını açıp aşağıdaki satırı yaz
  ```cmd
   terraform -version
   ```
   Sürüm bilgisi aşağıdaki gibi görülüyorsa işlem tamamdır.

   <img width="305" height="74" alt="image" src="https://github.com/user-attachments/assets/8ec05941-0cea-4aee-82ff-fd32acaf2ff9" />

   ## Proje Oluşturma

   VS Code içinde şu 3 dosyayı oluştur:

   ``` bash
mkdir terraform-github
cd terraform-github    
   ```

```css
terraform-github/
├── main.tf
├── variables.tf
└── terraform.tfvars

```

main.tf
```hcl
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

provider "github" {
  token = var.github_token
  owner = var.github_owner
}

resource "github_repository" "example_repo" {
  name        = "terraform-managed-repo"
  description = "Repository managed by Terraform via VS Code"
  visibility  = "private"
  has_issues  = true
  has_wiki    = false
}

```

variables.tf

```hcl
variable "github_token" {
  type        = string
  description = "GitHub Personal Access Token"
  sensitive   = true
}

variable "github_owner" {
  type        = string
  description = "GitHub account or organization name"
}
```
terraform.tfvars

```hcl
github_token = "ghp_xxxxxxx"     # Buraya token'ını yaz
github_owner = "kullanıcı-adın"  # GitHub kullanıcı adın veya organizasyon adın

```
### 🧩 Githubtan Token Alma İşlemleri

- GitHub’a gir:
  Settings → Developer settings → Personal Access Tokens → Tokens (classic)
  
- Yeni token oluştur:
  “Generate new token (classic)” de.
  
- Ad ver: terraform-github
  Süre: “No expiration” (veya 90 days de olabilir)

- ✅Aşağıdaki izinlerin işaretle

    - repo

    - admin:repo_hook

    - read:org

- Oluştur dedikten sonra çıkan ghp_.... token’ı kopyala (sonradan göremeyeceksin).

### Token'ı Güvenli Tutmak için

VS Code’da .gitignore dosyasına şu satırı ekle:

terraform.tfvars
.terraform/

### Terraform Komutları
Terraformu çalıştırmak için VS Code’un kendi terminalini aç ve sırasıyla şu komutları çalıştır:

```bash
terraform init
terraform plan
terraform apply

```

apply sırasında senden “yes” onayı isteyecek — yaz ve Enter’a bas.

yeni repo GitHub hesabında oluşacak:
Adı: terraform-managed-repo




   
