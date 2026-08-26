# Domain Yapılandırması

## 1. Genel Bilgiler

Bu dosyada kurumsal ağ altyapısı içerisinde kullanılan Active Directory Domain Services (AD DS) yapılandırması açıklanmaktadır.

Domain Controller, kullanıcı ve bilgisayar hesaplarının merkezi olarak yönetilmesini, kimlik doğrulama işlemlerinin gerçekleştirilmesini ve ağ içerisindeki istemcilerin domaine dahil edilmesini sağlar.

---

## 2. Domain Controller

| Özellik         | Değer                            |
| --------------- | -------------------------------- |
| Sunucu Adı      | DC01                             |
| Sunucu Rolü     | Domain Controller                |
| İşletim Sistemi | Windows Server                   |
| Domain          | corp.local                       |
| Rol             | Active Directory Domain Services |
| DNS             | Active Directory ile entegre     |
| IP Adresi       | Statik IP                        |

---

## 3. Active Directory Domain Services

Domain Controller üzerinde Active Directory Domain Services (AD DS) rolü kullanılmıştır.

AD DS sayesinde aşağıdaki işlemler merkezi olarak gerçekleştirilebilir:

* Kullanıcı hesaplarının yönetilmesi
* Bilgisayar hesaplarının yönetilmesi
* Kullanıcıların gruplara atanması
* Yetkilendirme işlemlerinin yapılması
* Group Policy uygulanması
* Domain ortamında kimlik doğrulama
* Ağ kaynaklarına erişimin merkezi olarak yönetilmesi

---

## 4. Domain Yapısı

Proje kapsamında kullanılan domain:

`corp.local`

Domain yapısında temel olarak bir Domain Controller ve domaine bağlı istemci bilgisayarlar bulunmaktadır.

Örnek yapı:

```text
corp.local
│
├── Users
│   ├── Kullanıcı hesapları
│   └── Yönetici hesapları
│
├── Computers
│   └── Domaine bağlı bilgisayarlar
│
└── Domain Controllers
    └── DC01
```

---

## 5. DNS Yapılandırması

Active Directory ortamının düzgün çalışabilmesi için DNS yapılandırması önemlidir.

Domain Controller aynı zamanda DNS hizmetini sağlamaktadır.

İstemci bilgisayarların DNS sunucusu olarak Domain Controller'ın IP adresini kullanması gerekir.

Örnek:

```text
Client DNS
     │
     ▼
DC01
     │
     └── DNS
          │
          └── corp.local
```

---

## 6. Kullanıcı Yönetimi

Active Directory üzerinde kullanıcı hesapları merkezi olarak yönetilir.

Kullanıcı hesapları oluşturulurken aşağıdaki bilgiler kullanılabilir:

* Kullanıcı adı
* Ad ve soyad
* Parola
* Grup üyelikleri
* Yetkiler
* Hesap durumu

Yönetici hesapları ile standart kullanıcı hesaplarının yetkileri birbirinden ayrılmalıdır.

---

## 7. Güvenlik

Domain ortamında güvenliği artırmak amacıyla:

* Güçlü parola politikaları uygulanmalıdır.
* Gereksiz yönetici yetkileri verilmemelidir.
* Kullanıcılar ihtiyaç duydukları gruplara dahil edilmelidir.
* Domain Controller yalnızca yetkili kullanıcılar tarafından yönetilmelidir.
* Group Policy kullanılarak merkezi güvenlik kuralları uygulanmalıdır.

---

## 8. İstemcilerin Domaine Katılması

Windows istemci bilgisayarların `corp.local` domainine dahil edilmesi için istemcinin DNS yapılandırması Domain Controller'ı gösterecek şekilde ayarlanmalıdır.

Daha sonra Windows sistem ayarlarından bilgisayar domain ortamına dahil edilir.

Örnek:

```text
Bilgisayar
   │
   ▼
DNS → DC01
   │
   ▼
corp.local
   │
   ▼
Domain'e Katılım
```

---

## 9. Kontrol ve Test

Domain yapılandırmasının doğru çalıştığını kontrol etmek için aşağıdaki işlemler gerçekleştirilebilir:

# Sunucu Yapılandırması

## 1. Genel Bilgiler

Bu dosyada kurumsal ağ altyapısında kullanılan sunucuların temel yapılandırmaları açıklanmaktadır.

Sunucular ağ içerisinde merkezi servislerin sağlanması ve istemci bilgisayarların yönetilmesi amacıyla kullanılmaktadır.

---

## 2. DC01 Sunucusu

| Özellik         | Değer                |
| --------------- | -------------------- |
| Sunucu Adı      | DC01                 |
| İşletim Sistemi | Windows Server       |
| Sunucu Rolü     | Domain Controller    |
| Domain          | corp.local           |
| IP Yapısı       | Statik IP            |
| DNS             | Active Directory DNS |

DC01 sunucusu ağ içerisindeki temel merkezi sunucu olarak kullanılmaktadır.

---

## 3. Sunucu Görevleri

DC01 üzerinde aşağıdaki temel servisler bulunmaktadır:

* Active Directory Domain Services
* DNS Server
* Domain Controller hizmetleri
* Kullanıcı ve bilgisayar hesaplarının yönetimi
* Domain kimlik doğrulama işlemleri

---

## 4. IP Yapılandırması

Sunucunun IP adresinin statik olarak yapılandırılması önerilmektedir.

Örnek yapı:

```text
IP Adresi      : 192.168.1.x
Alt Ağ Maskesi : 255.255.255.0
Ağ Geçidi      : 192.168.1.1
DNS            : DC01 IP adresi
```

> Kullanılan gerçek IP adresleri proje ortamındaki yapılandırmaya göre değiştirilmelidir.

---

## 5. Sunucu Adlandırması

Sunucu adı:

```text
DC01
```

Sunucu isimlendirmesinde anlaşılır ve standart isimlendirme kullanılması ağ yönetimini kolaylaştırır.

---

## 6. Windows Server Yapılandırması

Windows Server kurulumundan sonra temel yapılandırma işlemleri gerçekleştirilir.

Bu işlemler:

1. Bilgisayar adının değiştirilmesi
2. Statik IP adresinin verilmesi
3. DNS yapılandırmasının yapılması
4. Gerekli Windows Server rollerinin kurulması
5. Active Directory Domain Services rolünün kurulması
6. Domain Controller yapılandırmasının yapılması
7. DNS servisinin kontrol edilmesi

---

## 7. Sunucu Güvenliği

Sunucu güvenliği için aşağıdaki uygulamalar kullanılmalıdır:

* Güçlü yönetici parolası
* Gereksiz servislerin kapatılması
* Windows Firewall kullanımı
* Güncellemelerin düzenli yapılması
* Yönetici hesaplarının sınırlandırılması
* Yetkisiz erişimlerin engellenmesi

---

## 8. Servis Kontrolleri

Sunucu üzerindeki servislerin çalışıp çalışmadığını kontrol etmek için Windows Server yönetim araçları kullanılabilir.

Özellikle aşağıdaki servislerin kontrol edilmesi önemlidir:

```text
Active Directory Domain Services
DNS Server
Netlogon
Kerberos Key Distribution Center
```

---

## 9. Kontrol ve Doğrulama Testleri
Domain yapılandırmasını ve isim çözümlemesini doğrulamak için kullanılan test komutları:

Windows ve DC Tarafında:
- ipconfig /all
- nslookup corp.local
- nltest /dsgetdc:corp.local
- whoami

Ubuntu Tarafında:
- ping -c 4 corp.local
- realm list
- id Administrator@corp.local

---

## 10. Sonuç

Sunucu yapılandırması tamamlandıktan sonra DC01, kurumsal ağ ortamında merkezi kimlik doğrulama, DNS ve Active Directory yönetimi için kullanılabilir hale gelir.


Ayrıca istemci bilgisayar üzerinden domain kullanıcı hesabıyla oturum açılması test edilmelidir.

---

## 10. Sonuç

Active Directory Domain Services kullanılarak ağ içerisindeki kullanıcı, bilgisayar ve güvenlik yönetiminin merkezi hale getirilmesi amaçlanmıştır.

Bu yapı kurumsal ağlarda yönetilebilirlik, güvenlik ve merkezi kimlik doğrulama açısından önemli bir altyapı sağlar.
