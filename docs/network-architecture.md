# Ağ Mimarisi

## 1. Genel Bakış

Bu proje, temel bir kurumsal ağ altyapısının sanal ortamda oluşturulmasını ve merkezi olarak yönetilmesini amaçlamaktadır.

Ağ içerisinde sunucu ve istemci bilgisayarların birbirleriyle haberleşmesi ve Active Directory üzerinden merkezi olarak yönetilmesi sağlanmaktadır.

---

## 2. Ağ Topolojisi
Temel ağ yapısı aşağıdaki gibidir:

```text
                 İnternet / Ağ Geçidi (192.168.231.2)
                                 │
                                 │
                   Sanal Ağ (192.168.231.0/24)
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
       DC01                    DHCP01                CLIENT-W11 / SRV-LNX
  Windows Server          Windows Server (Klon)     Windows 11 / Ubuntu
  192.168.231.150         192.168.231.151         192.168.231.160 / .170
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                            corp.local

---

## 3. DC01

DC01 ağ içerisindeki merkezi sunucudur.

Görevleri:

* Domain Controller
* Active Directory
* DNS Sunucusu
* Kullanıcı yönetimi
* Bilgisayar yönetimi
* Merkezi Kimlik doğrulama

---

## 4. CLIENT-W11

* CLIENT-W11: Domain ortamına dahil edilen Windows 11 istemci bilgisayardır.

* SRV-LNX: realmd ve sssd ile domain ortamına bağlanan Ubuntu 22.04 istemci/sunucudur.

* DHCP01: Dinamik IP dağıtımını üstlenen klonlanmış DHCP sunucusudur.

İstemciler:

* DC01 üzerinden DNS çözümlemesi kullanır.

* corp.local domainine bağlıdır.

* Domain kullanıcılarıyla oturum açabilir.

* Active Directory üzerinden uygulanan grup politikalarını (GPO) alabilir.

---

## 5. IP Adresleme

Ağ içerisinde IP adreslerinin düzenli şekilde yönetilmesi gerekir.

Cihaz / Servis |	              Rol	                  |    IP Adresi     |	DNS Sunucusu
DC01	       |Primary Domain Controller	              |192.168.231.150	 |127.0.0.1 / 192.168.231.150
DHCP01         |DHCP Sunucusu (Klon)	                  |192.168.231.151	 |192.168.231.150
CLIENT-W11	   |Windows İstemci                           |	192.168.231.160	 |192.168.231.150
SRV-LNX        |Ubuntu İstemci	                          |192.168.231.170	 |192.168.231.150
Gateway	       |Sanal Ağ Geçidi	                          |192.168.231.2	 |        -

---

## 6. DNS Mimarisi

DNS, domain ortamının temel bileşenlerinden biridir.

İstemci bilgisayarların DNS sorguları öncelikle DC01 üzerinde çalışan DNS servisine yönlendirilir.

CLIENT-W11 / SRV-LNX
     │
     │ DNS Sorgusu (192.168.231.150)
     ▼
    DC01
     │
     ▼
DNS Server
     │
     ▼
corp.local

---

## 7. Active Directory Mimarisi

Active Directory merkezi kullanıcı ve bilgisayar yönetimi sağlar.
                    corp.local
                        │
                    ┌───┴───┐
                    │       │
                  Users   Computers
                    │       │
                    │       ├── CLIENT-W11
                    │       └── SRV-LNX
                    │
                    └── Kullanıcı Hesapları (user1, vb.)

---

## 8. Ağ İletişimi

Tüm sunucu ve istemciler aynı sanal ağ (192.168.231.0/24) içerisinde bulunmaktadır.
Temel iletişim protokolleri:

İstemciler (Windows / Linux)
     │
     ├── DNS (Port 53) ──────► DC01 (İsim Çözümleme)
     │
     ├── Kerberos (Port 88) ─► DC01 (Kimlik Doğrulama)
     │
     ├── LDAP (Port 389) ────► DC01 (Dizin Sorguları)
     │
     ├── SMB (Port 445) ─────► DC01 (GPO ve Dosya Paylaşımı)
     │
     └── DHCP (Port 67/68) ──► DHCP01 (IP Dağıtımı)DC01
     
```

Bu servisler Active Directory ortamının çalışmasında önemli rol oynar.

---

## 9. Güvenlik

Ağ güvenliği için:

* Kullanıcı yetkileri sınırlandırılmalıdır.
* Güçlü parola politikaları uygulanmalıdır.
* Windows Firewall aktif tutulmalıdır.
* Gereksiz ağ servisleri kullanılmamalıdır.
* Domain Controller erişimi yetkili kullanıcılarla sınırlandırılmalıdır.
* Ağ trafiği ve sistem günlükleri gerektiğinde kontrol edilmelidir.

---

## 10. Sonuç

Kurulan ağ mimarisinde DC01 merkezi sunucu, CLIENT-W11 ise domain istemcisi olarak görev yapmaktadır.

Active Directory ve DNS servislerinin merkezi olarak DC01 üzerinde bulunması sayesinde kullanıcı ve istemci bilgisayar yönetimi merkezi hale getirilmektedir.
