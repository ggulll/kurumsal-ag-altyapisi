# Ağ Yapılandırması

## Ağ Genel Bakışı

Bu proje kapsamında VMware üzerinde sanallaştırılmış bir laboratuvar ağı oluşturulmuştur.

Laboratuvar ortamında aşağıdaki dört sanal makine bulunmaktadır:

- DC01
- DHCP01
- CLIENT-W11
- SRV-LNX

## IP Adresleme Planı

| Sanal Makine | Rol | İşletim Sistemi | IP Adresi |
|---|---|---|---|
| DC01 | Domain Controller, AD DS, DNS | Windows Server 2022 | 192.168.231.150 |
| DHCP01 | DHCP Sunucusu | Windows Server 2022 | 192.168.231.151 |
| CLIENT-W11 | Windows İstemci | Windows 11 Pro | 192.168.231.160 |
| SRV-LNX | Linux Sunucusu / İstemci | Ubuntu 22.04 | 192.168.231.170 |

## Ağ Ayarları

- Alt Ağ Maskesi: 255.255.255.0
- Varsayılan Ağ Geçidi: 192.168.231.2
- DNS Sunucusu: 192.168.231.150

## DHCP Kapsamı

İstemcilere otomatik IP adresi dağıtmak amacıyla DHCP kapsamında aşağıdaki IP aralığı tanımlanmıştır:

- Başlangıç IP: 192.168.231.160
- Bitiş IP: 192.168.231.200
- DNS Sunucusu: 192.168.231.150
- Varsayılan Ağ Geçidi: 192.168.231.2

## VMware Ağ Yapısı

Laboratuvar ortamında iki farklı sanal ağ türü kullanılmıştır:

- **NAT:** İnternet erişimi için kullanılır.
- **Host-Only:** İzole test ortamında sanal makinelerin birbiriyle iletişim kurması için kullanılır.

## DNS Yapılandırması

DC01, Active Directory ortamının DNS sunucusu olarak yapılandırılmıştır.

İstemci makinelerde DNS sunucusu olarak DC01'in IP adresi kullanılır:

`192.168.231.150`

Bu yapılandırma sayesinde istemci makinelerin domain adını çözümlemesi ve Active Directory ortamıyla iletişim kurması sağlanır.