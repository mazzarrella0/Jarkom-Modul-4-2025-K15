# Jarkom-Modul-4-2025-K15

# Jarkom - Konfigurasi Jaringan VLSM & Agregasi CIDR

Dokumentasi ini menjelaskan konfigurasi jaringan GNS3 menggunakan router Ubuntu, berdasarkan alokasi IP VLSM dan agregasi rute CIDR.

**Base IP:** `10.71.0.0`
**Metode:**
1.  **Alokasi:** VLSM (Variable Length Subnet Masking) untuk membagi IP sesuai kebutuhan host.
2.  **Routing:** Static Routing dengan Agregasi CIDR (Classless Inter-Domain Routing) untuk supernetting.

---

## Topologi Jaringan

Topologi dibagi menjadi beberapa cabang utama yang terhubung melalui router-router inti.

<img width="1481" height="678" alt="Screenshot 2025-11-13 203618" src="https://github.com/user-attachments/assets/48fec561-5732-4784-a38c-e0a9c31c60f4" />

---

## 1. Pembagian Subnetting (Alokasi VLSM)

Berikut adalah tabel alokasi IP yang digunakan untuk mengkonfigurasi setiap interface LAN dan WAN di topologi, berdasarkan file `image_9eb1a8.png`.

| Subnet | Network ID | Netmask | Broadcast | Range IP |
| :--- | :--- | :--- | :--- | :--- |
| LAN_Switch7 (Mirdain+Arthedain) | 10.71.0.0 | 255.255.252.0 | 10.71.3.255 | 10.71.0.1 - 10.71.3.254 |
| LAN_Switch3 (Beacon+Silmarils) | 10.71.4.0 | 255.255.252.0 | 10.71.7.255 | 10.71.4.1 - 10.71.7.254 |
| LAN_Switch8 (Melkor+Khazad) | 10.71.8.0 | 255.255.254.0 | 10.71.9.255 | 10.71.8.1 - 10.71.9.254 |
| LAN_Switch9 (Balrog+Gothmog+Thranduil) | 10.71.10.0 | 255.255.254.0 | 10.71.11.255 | 10.71.10.1 - 10.71.11.254 |
| LAN_Switch10 (Shadow+Anarion+Lindon) | 10.71.12.0 | 255.255.254.0 | 10.71.13.255 | 10.71.12.1 - 10.71.13.254 |
| LAN_Switch4 (Mirkwood+Morgul) | 10.71.14.0 | 255.255.255.0 | 10.71.14.255 | 10.71.14.1 - 10.71.14.254 |
| LAN_Switch5 (Palantir+Edhil) | 10.71.15.0 | 255.255.255.128 | 10.71.15.127 | 10.71.15.1 - 10.71.15.126 |
| LAN_Switch2 (Erendis+Elrond) | 10.71.15.128 | 255.255.255.192 | 10.71.15.191 | 10.71.15.129 - 10.71.15.190 |
| LAN_Switch12 (Doriath+Arnor) | 10.71.15.192 | 255.255.255.192 | 10.71.15.255 | 10.71.15.193 - 10.71.15.254 |
| LAN_Switch11 (Imrahil+Gwaith+Utumno) | 10.71.16.0 | 255.255.255.192 | 10.71.16.63 | 10.71.16.1 - 10.71.16.62 |
| LAN_Switch6 (IronCrown+Grond+Hobbitton) | 10.71.16.64 | 255.255.255.224 | 10.71.16.95 | 10.71.16.65 - 10.71.16.94 |
| WAN_Switch1 (Amroth+Morgoth+Throne) | 10.71.16.96 | 255.255.255.248 | 10.71.16.103 | 10.71.16.97 - 10.71.16.102 |
| LAN_Erebor | 10.71.16.104 | 255.255.255.252 | 10.71.16.107 | 10.71.16.105 - 10.71.16.106 |
| WAN (Valinor-Valmar) (Switch13) | 10.71.16.108 | 255.255.255.248 | 10.71.16.115 | 10.71.16.109 - 10.71.16.114 |
| WAN (Valinor-Fornost) | 10.71.16.120 | 255.255.255.252 | 10.71.16.123 | 10.71.16.121 - 10.71.16.122 |
| WAN (Fornost-Amonsul) | 10.71.16.124 | 255.255.255.252 | 10.71.16.127 | 10.71.16.125 - 10.71.16.126 |
| WAN (Amonsul-Eregion) | 10.71.16.128 | 255.255.255.252 | 10.71.16.131 | 10.71.16.129 - 10.71.16.130 |
| WAN (Amonsul-Minastir) | 10.71.16.132 | 255.255.255.252 | 10.71.16.135 | 10.71.16.133 - 10.71.16.134 |
| WAN (Amonsul-NAT1) | 10.71.16.136 | 255.255.255.252 | 10.71.16.139 | 10.71.16.137 - 10.71.16.138 |
| WAN (Eregion-MirkwoodRTR) | 10.71.16.140 | 255.255.255.252 | 10.71.16.143 | 10.71.16.141 - 10.71.16.142 |
| WAN (Minastir-Amroth) | 10.71.16.144 | 255.255.255.252 | 10.71.16.147 | 10.71.16.145 - 10.71.16.146 |
| WAN (Minastir-Anor) | 10.71.16.148 | 255.255.255.252 | 10.71.16.151 | 10.71.16.149 - 10.71.16.150 |
| WAN (Anor-BeaconRTR) | 10.71.16.152 | 255.255.255.252 | 10.71.16.155 | 10.71.16.153 - 10.71.16.154 |
| WAN (Anor-Numenor) | 10.71.16.156 | 255.255.255.252 | 10.71.16.159 | 10.71.16.157 - 10.71.16.158 |
| WAN (Anor-Mordor) | 10.71.16.160 | 255.255.255.252 | 10.71.16.163 | 10.71.16.161 - 10.71.16.162 |
| WAN (Numenor-Gudur) | 10.71.16.164 | 255.255.255.252 | 10.71.16.167 | 10.71.16.165 - 10.71.16.166 |
| WAN (Numenor-MirdainRTR) | 10.71.16.168 | 255.255.255.252 | 10.71.16.171 | 10.71.16.169 - 10.71.16.170 |
| WAN (Numenor-Mordor) | 10.71.16.172 | 255.255.255.252 | 10.71.16.175 | 10.71.16.173 - 10.71.16.174 |
| WAN (Gudur-PalantirRTR) | 10.71.16.176 | 255.255.255.252 | 10.71.16.179 | 10.71.16.177 - 10.71.16.178 |
| WAN (Gudur-GrondRTR) | 10.71.16.180 | 255.255.255.252 | 10.71.16.183 | 10.71.16.181 - 10.71.16.182 |
| WAN (Mordor-Erain) | 10.71.16.184 | 255.255.255.252 | 10.71.16.187 | 10.71.16.185 - 10.71.16.186 |
| WAN (Erain-MelkorRTR) | 10.71.16.188 | 255.255.255.252 | 10.71.16.191 | 10.71.16.189 - 10.71.16.190 |
| WAN (Erain-BalrogRTR) | 10.71.16.192 | 255.255.255.252 | 10.71.16.195 | 10.71.16.193 - 10.71.16.194 |
| WAN (Morgoth-ElrondRTR) | 10.71.16.196 | 255.255.255.252 | 10.71.16.199 | 10.71.16.197 - 10.71.16.198 |

---

## 2. Penggabungan Subnet (Agregasi CIDR)

Subnet-subnet di atas kemudian diagregasi secara hierarkis (bottom-up) untuk route summarization.

### Level I
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| B1 | A19 | /24 | A21 | /29 | /22 |
| B2 | A18 | /27 | A20 | /27 | /21 |
| B3 | A16 | /25 | A17 | /28 | /24 |
| B4 | A14 | /23 | A15 | /23 | /22 |
| B5 | A6 | /22 | A4 | /22 | /21 |
| B6 | A5 | /30 | A8 | /26 | /25 |

### Level II
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| C1 | B1 | /22 | B2 | /21 | /21 |
| C2 | B3 | /24 | A12 | /30 | /23 |
| C3 | B4 | /22 | A13 | /30 | /21 |
| C4 | B5 | /21 | A23 | /30 | /20 |
| C5 | B6 | /25 | A7 | /29 | /24 |

### Level III
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| D1 | C1 | /21 | A22 | /30 | /20 |
| D2 | C2 | /23 | A10 | /22 | /21 |
| D3 | C3 | /21 | A11 | /30 | /20 |
| D4 | C4 | /20 | C5 | /24 | /19 |

### Level IV
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E1 | D2 | /21 | D3 | /20 | /19 |
| E2 | D4 | /19 | A3 | /30 | /18 |

### Level V
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| F1 | E1 | /19 | A9 | /30 | /18 |

### Level VI
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| G1 | F1 | /18 | A2 | /25 | /17 |

### Level VII
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| H1 | G1 | /17 | A1 | /30 | /16 |

### Level VIII
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| I1 | H1 | /16 | E2 | /18 | /15 |

### Level IX (Root)
| Subnet | Gabungan dari Subnet 1 | Netmask 1 | Gabungan dari Subnet 2 | Netmask 2 | Netmask Akhir |
| :--- | :--- | :--- | :--- | :--- | :--- |
| J1 | I1 | /15 | D1 | /20 | /14 |

---

## 3. Pohon (Tree) Agregasi CIDR

Berikut adalah representasi visual dari tabel agregasi subnet di atas.

*(Silakan ganti placeholder di bawah ini dengan gambar pohon CIDR Anda)*

<img width="1075" height="605" alt="Screenshot 2025-11-16 182747" src="https://github.com/user-attachments/assets/63fd8806-a0db-447b-9a2f-9d8f1b7685fe" />


---

## 4. Konfigurasi Router (GNS3 - Ubuntu)

### 4.1. Router: Valinor

**File `/etc/network/interfaces`:**

```ini
# The loopback network interface
auto lo
iface lo inet loopback

# LAN Gabungan (Switch10) - 10.71.12.0/23
auto eth1
iface eth1 inet static
    address 10.71.12.1
    netmask 255.255.254.0

# WAN Bersama (Switch13) - 10.71.16.104/29
auto eth0
iface eth0 inet static
    address 10.71.16.109
    netmask 255.255.255.248
```

Static Routes :
```
# Aktifkan IP Forwarding
sysctl -w net.ipv4.ip_forward=1

# Rute ke LAN Switch11 (via Valmar)
ip route add 10.71.16.0/26 via 10.71.16.110

# Rute ke LAN Switch12 (via Valmar)
ip route add 10.71.15.192/26 via 10.71.16.110
```

### 4.2 Router:Valmar
**File `/etc/network/interfaces`:**
```
# The loopback network interface
auto lo
iface lo inet loopback

# LAN Gabungan (Switch12) - 10.71.15.192/26
auto eth1
iface eth1 inet static
    address 10.71.15.193
    netmask 255.255.255.192

# LAN Gabungan (Switch11) - 10.71.16.0/26
auto eth2
iface eth2 inet static
    address 10.71.16.1
    netmask 255.255.255.192

# WAN Bersama (Switch13) - 10.71.16.104/29
auto eth0
iface eth0 inet static
    address 10.71.16.110
    netmask 255.255.255.248
```

### 5. Strategi Routing
Strategi routing yang digunakan adalah Static Routing.

- Core-to-Core (Contoh: Valinor <-> Valmar): Rute spesifik (ip route add <network> via <next-hop>) digunakan untuk memberitahu router cara menjangkau LAN di seberangnya.
- Edge-to-Core (Contoh: Gudur -> Numenor): Rute default (ip route add 0.0.0.0/0 via <next-hop>) digunakan. Router "tepi" (seperti Gudur) tidak perlu tahu seluruh topologi; ia hanya perlu tahu ke mana harus mengirim semua trafik yang tidak dikenal (yaitu ke router di atasnya, Numenor).

### 6. Perintah Pengujian & Troubleshooting
Perintah-perintah berikut sangat penting untuk memverifikasi konfigurasi dan mencari masalah.

```Bash
# 1. Cek konfigurasi IP dan status interface
# (Apakah IP sudah benar? Apakah interface 'UP'?)
ip addr show

# 2. Cek tabel routing
# (Apakah static route/default route sudah masuk?)
ip route show

# 3. Tes konektivitas dasar
# (Ping ke gateway, lalu ping ke router/client seberang)
ping -c 4 <IP_TUJUAN>

# 4. Lacak jalur paket
# (Untuk melihat di router mana paket berhenti)
traceroute <IP_TUJUAN>

# 5. Cek status IP forwarding
# (Harus bernilai '1')
cat /proc/sys/net/ipv4/ip_forward

# 6. Aktifkan IP forwarding (jika mati)
sysctl -w net.ipv4.ip_forward=1
```
