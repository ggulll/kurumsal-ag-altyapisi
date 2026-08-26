# Kurulum Dokümantasyonu

## 1. Amaç

Bu dokümanın amacı, kurumsal ağ altyapısı projesinde kullanılan sanal makinelerin ve temel ağ bileşenlerinin kurulum adımlarını açıklamaktır.

Proje kapsamında sanallaştırma ortamı kullanılarak Windows Server ve Windows 11 işletim sistemleri kurulmuş ve bu sistemlerin birbiriyle iletişim kurması sağlanmıştır.

---

## 2. Gereksinimler

Kurulum için aşağıdaki bileşenlere ihtiyaç duyulmaktadır:

* VMware Workstation
* Windows Server ISO dosyası
* Windows 11 ISO dosyası
* Yeterli RAM ve disk alanı
* Sanal ağ bağlantısı

---

## 3. Sanal Makinelerin Oluşturulması

Proje kapsamında laboratuvar ortamında dört sanal makine planlanmış ve kullanılmıştır:

```text
Kurumsal Ağ Laboratuvarı
│
├── DC01 (Windows Server 2022 - Primary Domain Controller & DNS)
├── DHCP01 (Windows Server 2022 - DHCP Sunucusu / Klon VM)
├── CLIENT-W11 (Windows 11 Pro - Windows İstemcisi)
└── SRV-LNX (Ubuntu 22.04 LTS - Linux Sunucu / İstemcisi)
```

### DC01

DC01, ağ içerisindeki Domain Controller olarak görev yapmaktadır.

Temel görevleri:

* Active Directory Domain Services
* DNS
* Kullanıcı yönetimi
* Bilgisayar yönetimi
* Domain kimlik doğrulama

### CLIENT-W11

CLIENT-W11, domain ortamına dahil edilen Windows 11 istemci bilgisayardır.

---

## 4. Windows Server Kurulumu

İlk olarak VMware üzerinde DC01 isimli sanal makine oluşturulur.
Windows Server 2022 ISO dosyası sanal makineye bağlanarak Desktop Experience seçeneğiyle işletim sistemi kurulumu gerçekleştirilir.

Kurulum tamamlandıktan sonra:

1. Bilgisayar adı DC01 olarak değiştirilir.

2. Statik IP adresi (192.168.231.150 / 255.255.255.0, Gateway: 192.168.231.2) yapılandırılır.

3. DNS ayarları yapılır (Tercih edilen DNS: 127.0.0.1 veya 192.168.231.150).

4. VMware Tools kurulur ve gerekli Windows Server güncellemeleri uygulanır.

5. Active Directory Domain Services rolü kurulmaya hazırlanır.

---

## 5. Active Directory Domain Services Kurulumu

Windows Server üzerinde **Server Manager** açılır.

Aşağıdaki yol izlenir:

```text 
Server Manager
    ↓
Add Roles and Features
    ↓
Active Directory Domain Services & DNS Server
    ↓
Install
```

Rol kurulumu tamamlandıktan sonra sunucu Promote this server to a domain controller seçeneğiyle Domain Controller olarak yapılandırılır.

---

## 6. Domain Oluşturulması

Active Directory yapılandırması sırasında yeni bir domain oluşturulur.

Proje kapsamında kullanılan domain:

```text 
corp.local
```

olarak belirlenmiştir.

Domain Controller yapılandırması tamamlandıktan sonra DC01 üzerinde Active Directory ve DNS servisleri kullanılabilir hale gelir.

---

## 7. DNS Yapılandırması

Active Directory ortamının düzgün çalışabilmesi için DNS yapılandırmasının doğru yapılması gerekir.

DC01 üzerinde DNS servisi çalıştırılır.

DNS Manager üzerinden aşağıdaki alan kontrol edilir:

```text 
Forward Lookup Zones
└── corp.local
```

İstemci bilgisayarların (Windows ve Linux) DNS sunucusu olarak DC01'in IP adresi (192.168.231.150) kullanılmalıdır.

---

## 8. Windows 11 Kurulumu

İkinci sanal makine Windows 11 işletim sistemi için oluşturulur.

Sanal makinenin adı:

```text 
CLIENT-W11
```

olarak belirlenebilir.

Windows 11 kurulumu tamamlandıktan sonra ağ bağlantısı kontrol edilir ve statik IP (192.168.231.160) tanımlanır.

---

## 9. İstemcinin DNS Yapılandırması

CLIENT-W11 üzerinde ağ ayarları açılır.

DNS sunucusu olarak DC01'in IP adresi girilir.


```text 
DNS Sunucusu: 192.168.231.150
```

Bu yapılandırma, istemci bilgisayarın `corp.local` domainini doğru şekilde bulabilmesi için gereklidir.

---

## 10. İstemcinin Domaine Katılması

CLIENT-W11 bilgisayarının sistem ayarlarından (Gelişmiş Sistem Ayarları > Bilgisayar Adı > Değiştir) domain üyeliği yapılandırılır.

Domain Adı: corp.local

Yetkili domain yöneticisi (corp\Administrator) bilgileri girildikten sonra bilgisayar yeniden başlatılır.
Yeniden başlatma sonrasında Windows 11 istemcisi domain ortamına başarıyla dahil edilmiş olur.

---

## 11. Kullanıcı Hesabı Oluşturulması

Active Directory Users and Computers aracı kullanılarak domain kullanıcıları oluşturulabilir.

Örnek yapı:

```text 
corp.local
└── CorpUsers
    ├── admin
    ├── user1
    └── user2
```

Kullanıcıların görevlerine göre uygun güvenlik gruplarına atanması önerilir.

---

## 12. Kurulum Sonrası Testler

Kurulum tamamlandıktan sonra ağ bağlantısının ve domain servislerinin doğru çalıştığı kontrol edilir.

### IP Yapılandırması

```powershell 
ipconfig /all
```

### DC01 Bağlantı Testi

```powershell 
ping 192.168.231.150
```

### Domain DNS Testi

```powershell 
nslookup corp.local
```

Bu testlerin başarılı olması, istemci ile Domain Controller arasındaki temel iletişimin çalıştığını gösterir.

---

## 13. Domain Oturum Açma Testi

CLIENT-W11 yeniden başlatıldıktan sonra oluşturulan domain kullanıcı hesabıyla oturum açılması denenir.

Örnek:

```text 
CORP\user1 veya user1@corp.local
```

Domain hesabıyla başarılı şekilde oturum açılması, Active Directory ve istemci bağlantısının çalıştığını gösterir.

---

## 14. Kurulum Sonucu

Kurulum tamamlandığında temel kurumsal ağ altyapısı aşağıdaki yapıya ulaşır:

```text 
Sanal Ağ (192.168.231.0/24)
                               │
         ┌─────────────────────┴─────────────────────┐
         │                                           │
       DC01                                     CLIENT-W11
  Windows Server 2022                         Windows 11 Pro
  (192.168.231.150)                          (192.168.231.160)
         │                                           │
         │                                           │
         └─────────────────────┬─────────────────────┘
                               │
                           corp.local
                               │
                     Active Directory (AD DS)
                               │
                              DNS
```

Bu yapı sayesinde kullanıcı ve bilgisayarların merkezi olarak yönetilebildiği temel bir kurumsal domain ortamı oluşturulmuştur.
