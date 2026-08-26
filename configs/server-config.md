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

## 9. Bağlantı Testleri

Ağ bağlantısını kontrol etmek için:

```powershell
ipconfig /all
```

```powershell
ping 192.168.231.2
```

```powershell
ping 192.168.231.150
```

DNS kontrolü için:

```powershell
nslookup corp.local
```

komutları kullanılabilir.

---

## 10. Sonuç

Sunucu yapılandırması tamamlandıktan sonra DC01, kurumsal ağ ortamında merkezi kimlik doğrulama, DNS ve Active Directory yönetimi için kullanılabilir hale gelir.
