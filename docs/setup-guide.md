# Kurulum ve Yapılandırma Rehberi

## 1. Giriş

Bu rehber, kurumsal ağ altyapısının kurulmasından sonra temel yapılandırmaların hangi sırayla yapılacağını açıklamaktadır.

Kurulumun aşağıdaki sıra ile gerçekleştirilmesi önerilmektedir:

'''text
1. Sanal ağın hazırlanması (VMware NAT / Subnet: 192.168.231.0/24)
        ↓
2. DC01 kurulumu ve Statik IP yapılandırması (192.168.231.150)
        ↓
3. Active Directory (AD DS) kurulumu ve forest inşası (corp.local)
        ↓
4. DNS kontrolü ve isim çözümleme testleri
        ↓
5. DHCP01 klonlama, Sysprep ve DHCP Scope tanımlama
        ↓
6. CLIENT-W11 ve SRV-LNX (Ubuntu) istemcilerinin kurulumu
        ↓
7. İstemcilerde DNS yapılandırması (DNS -> DC01)
        ↓
8. İstemcilerin domaine katılması (Domain Join)
        ↓
9. Kullanıcı oluşturma ve parola politikası testi
        ↓
10. Grup İlkesi (GPO) yönetimi ve bağlantı testleri
'''

---

## 2. DC01 Yapılandırması

Windows Server kurulduktan sonra sunucunun adı:

```text
DC01
```

olarak belirlenir.

Ardından statik IP adresi yapılandırılır.

Örnek:

```text
DC01 Statik IP Ayarları:
- IP Adresi      : 192.168.231.150
- Alt Ağ Maskesi : 255.255.255.0
- Ağ Geçidi      : 192.168.231.2
- DNS            : 127.0.0.1 / 192.168.231.150
```

---

## 3. Domain Oluşturma

Active Directory Domain Services rolü kurulduktan sonra yeni bir forest oluşturulur.

Domain adı:

```text
corp.local
```

olarak belirlenebilir.

Kurulum tamamlandıktan sonra sunucu Domain Controller olarak çalışır.

---

## 4. DNS Kontrolü

DNS Manager açılarak domain bölgesi kontrol edilir.

Kontrol edilmesi gereken temel alan:

```text
Forward Lookup Zones
└── corp.local
```

İstemci bilgisayarın DNS ayarının DC01'i gösterdiğinden emin olunmalıdır.

---

## 5. Kullanıcı Hesabı Oluşturma

Active Directory Users and Computers üzerinden kullanıcı hesapları oluşturulabilir.

Örnek:

```text
corp.local
└── CorpUsers (OU)
    ├── admin
    ├── user1
    └── user2
```

Kullanıcıların görevlerine göre uygun güvenlik gruplarına atanması önerilir.

---

## 6. Windows 11 İstemci Yapılandırması

CLIENT-W11 üzerinde:

1. Bilgisayar adı belirlenir (CLIENT-W11).

2. Ağ bağlantısı kontrol edilir (192.168.231.160).

D3. NS sunucusu DC01 (192.168.231.150) olarak ayarlanır.

4. DC01'e bağlantı ve isim çözümlemesi test edilir:

ping 192.168.231.150
nslookup corp.local

---

## 7. Domaine Katılma

Windows 11 tarafında:

* Sistem Özellikleri > Bilgisayar Adı > Değiştir menüsünden etki alanı seçilir.

* Domain adı olarak corp.local girilir.

* Yetkili domain hesabı bilgileri girildikten sonra bilgisayar yeniden başlatılır.

Ubuntu Linux (SRV-LNX) tarafında:

* realmd ve sssd araçları kurularak sudo realm join --user=Administrator corp.local komutu çalıştırılır.irildikten sonra bilgisayar yeniden başlatılır.

---

## 8. Domain Oturum Açma Testi

Bilgisayar yeniden başlatıldıktan sonra domain kullanıcı hesabıyla oturum açılır.

Örnek Formatlar:

```text
* CORP\user1

* user1@corp.local
```

İlk girişte kullanıcının yeni parola belirlemesi istenir ve sistemin bu kuralı zorunlu tuttuğu doğrulanır.

---

## 9. Group Policy

Kurumsal ağ ortamında kullanıcı ve bilgisayarlara merkezi ayarlar uygulamak için Group Policy kullanılabilir.

Group Policy ile örneğin:

* Parola politikaları
* Güvenlik ayarları
* Kullanıcı kısıtlamaları
* Windows ayarları
* Yazılım yapılandırmaları

merkezi olarak yönetilebilir.

İstemcilerde ilkeleri hemen zorlamak için:

gpupdate /force

---

## 10. Son Kontroller

Kurulum tamamlandıktan sonra aşağıdaki doğrulamalar gerçekleştirilir:

* IP Kontrolü: ipconfig /all veya ip a

* DNS Kontrolü: nslookup corp.local

* Ping Testi: ping DC01 / ping 192.168.231.150

* Domain Bilgisi: whoami, nltest /dsgetdc:corp.local veya realm list

* Kullanıcı Testi: Domain kullanıcısıyla Windows 11 ve Ubuntu üzerinde oturum açılması.


---

## 11. Sorun Giderme

Domain bağlantısı çalışmıyorsa öncelikle aşağıdaki noktalar kontrol edilmelidir:

1. DC01 çalışıyor mu?
2. CLIENT-W11 ile DC01 aynı sanal ağda mı?
3. İstemcinin DNS adresi DC01'i gösteriyor mu?
4. `ping DC01` çalışıyor mu?
5. `nslookup corp.local` doğru sonuç veriyor mu?
6. Domain adı doğru yazılmış mı?
7. Kullanıcı hesabının yetkileri doğru mu?
8. Windows Firewall bağlantıyı engelliyor mu?

---

## 12. Sonuç

Bu rehberde kurumsal ağ altyapısının temel kurulum ve yapılandırma adımları açıklanmıştır.

Kurulum sonunda:

```text
DC01 (192.168.231.150)
             │
             ├── Active Directory Domain Services
             ├── DNS Server
             └── Domain Controller
                      │
                      ▼
                  corp.local
                      │
         ┌────────────┴────────────┐
         │                         │
         ▼                         ▼
    CLIENT-W11                  SRV-LNX
 (Windows İstemci)          (Ubuntu İstemci)
```

şeklinde temel bir domain ağı oluşturulmuş olur.
