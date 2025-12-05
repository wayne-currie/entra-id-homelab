# Microsoft Entra ID Homelab in Azure

> *Formerly known as Azure Active Directory*
---

## Project Steps

### 1. Create an Azure Virtual Network (VNet)

### 2. Create a Virtual Network Gateway VPN

### 3. Generate a Root and Client Certificates
Run Powershell as Administrator
```bash
# Step 1: Create a Self-Signed Root Certificate

$params = @{
    Type = 'Custom'
    Subject = 'CN=AzureRoot'
    KeySpec = 'Signature'
    KeyExportPolicy = 'Exportable'
    KeyUsage = 'CertSign'
    KeyUsageProperty = 'Sign'
    KeyLength = 2048
    HashAlgorithm = 'sha256'
    NotAfter = (Get-Date).AddMonths(24)
    CertStoreLocation = 'Cert:\CurrentUser\My' 
}

$cert = New-SelfSignedCertificate @params
```
```bash
# Client certificate
$clientParams = @{
    Type = 'Custom'
    Subject = 'CN=AzureClient'
    DnsName = 'AzureClient'
    KeySpec = 'Signature'
    KeyExportPolicy = 'Exportable'
    KeyLength = 2048
    HashAlgorithm = 'sha256'
    NotAfter = (Get-Date).AddMonths(18)
    CertStoreLocation = 'Cert:\CurrentUser\My'
    Signer = $cert
    TextExtension = @('2.5.29.37={text}1.3.6.1.5.5.7.3.2')
}
New-SelfSignedCertificate @clientParams
```

### 4. Configure Point-to-site in the Azure VPN Gateway

### 5. Deploy Windows Server 2022

I deployed a Windows Server 2022 virtual machine in Azure to serve as the foundation for the Active Directory environment.
<img width="1872" height="830" alt="image" src="https://github.com/user-attachments/assets/630c10e4-15bf-4b2c-9db7-40d91ee84d89" />



---
### 6. Configure Windows Server 2022 as a Domain Controller

I configured the Windows Server 2022 instance as a Domain Controller to manage authentication and authorization services.

**Services Configured:**
- Active Directory Domain Services (AD DS)
- DNS Server
- Domain Controller promotion
<img width="1877" height="722" alt="image" src="https://github.com/user-attachments/assets/4920f984-aeb5-4398-ac98-9e548ccc2e19" />

---
### 7. Connect Windows 11 Host to Domain using Point-to-site VPN


