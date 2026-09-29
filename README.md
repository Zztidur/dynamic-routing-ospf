# Dynamic Routing OSPF Multi-Site — Cisco Packet Tracer

Implementasi dynamic routing menggunakan Open Shortest Path First (OSPF) pada jaringan multi-site menggunakan Cisco Packet Tracer.

Project ini menghubungkan tiga site:

- Head Office (HQ)
- Cabang A
- Cabang B

Berbeda dengan static routing, pada project ini router menggunakan OSPF untuk bertukar informasi routing dan mempelajari network secara dinamis.

---

## 📌 Project Overview

Topologi terdiri dari tiga jaringan LAN dan dua koneksi WAN point-to-point.

```text
HQ LAN       : 192.168.10.0/24
Cabang A LAN : 192.168.20.0/24
Cabang B LAN : 192.168.30.0/24
```

Koneksi WAN:

```text
HQ ↔ Cabang A : 10.10.1.0/30
HQ ↔ Cabang B : 10.10.2.0/30
```

OSPF digunakan sebagai dynamic routing protocol untuk mempelajari jaringan remote.

---

## 🗺️ Network Topology

```text
                    +----------------+
                    |       HQ       |
                    | 192.168.10.0/24|
                    +---+--------+---+
                        |        |
                10.10.1.0/30   10.10.2.0/30
                        |        |
                        |        |
                 +------+--+  +--+------+
                 | Cabang A|  | Cabang B|
                 |192.168. |  |192.168. |
                 |20.0/24  |  |30.0/24  |
                 +---------+  +---------+
```

---

## 🌐 IP Addressing

### HQ

```text
GigabitEthernet0/0 : 192.168.10.1/24
Serial0/0/0        : 10.10.1.1/30
Serial0/0/1        : 10.10.2.1/30
```

### Cabang A

```text
GigabitEthernet0/0 : 192.168.20.1/24
Serial0/0/0        : 10.10.1.2/30
```

### Cabang B

```text
GigabitEthernet0/0 : 192.168.30.1/24
Serial0/0/0        : 10.10.2.2/30
```

---

## 📊 Network Summary

| Site | LAN Network | Gateway |
|---|---|---|
| HQ | 192.168.10.0/24 | 192.168.10.1 |
| Cabang A | 192.168.20.0/24 | 192.168.20.1 |
| Cabang B | 192.168.30.0/24 | 192.168.30.1 |

### WAN

| Link | Network | Router A | Router B |
|---|---|---|---|
| HQ ↔ Cabang A | 10.10.1.0/30 | 10.10.1.1 | 10.10.1.2 |
| HQ ↔ Cabang B | 10.10.2.0/30 | 10.10.2.1 | 10.10.2.2 |

---

# 🔄 Dynamic Routing with OSPF

Pada project ini OSPF digunakan untuk memungkinkan router bertukar informasi routing.

OSPF route dapat dikenali pada routing table menggunakan kode:

```text
O
```

---

## 🤝 OSPF Neighbor

Verifikasi OSPF neighbor dilakukan menggunakan:

```text
show ip ospf neighbor
```

Pada HQ terdapat dua neighbor:

```text
Neighbor ID     State
192.168.20.1    FULL
192.168.30.1    FULL
```

Hubungan tersebut menunjukkan:

```text
HQ ↔ Cabang A
HQ ↔ Cabang B
```

telah membentuk OSPF adjacency.

---

## 🛣️ OSPF Routing Table

Verifikasi routing table dilakukan menggunakan:

```text
show ip route
```

Pada HQ terdapat route OSPF:

```text
O 192.168.20.0/24 via 10.10.1.2
O 192.168.30.0/24 via 10.10.2.2
```

Artinya HQ memperoleh informasi mengenai network Cabang A dan Cabang B melalui OSPF.

---

## 🔍 OSPF Verification

Command yang digunakan:

```text
show ip ospf neighbor
```

Untuk melihat OSPF adjacency.

```text
show ip route
```

Untuk melihat routing table.

```text
show ip interface brief
```

Untuk memeriksa status interface.

```text
show running-config
```

Untuk memeriksa konfigurasi perangkat.

---

# 🧪 Connectivity Testing

Pengujian dilakukan dari PC pada masing-masing site.

## HQ Testing

Pengujian dari HQ menuju:

```text
192.168.10.1
192.168.20.10
192.168.30.10
```

Hasil:

| Destination | Packet Loss | Average |
|---|---:|---:|
| 192.168.10.1 | 0% | 0 ms |
| 192.168.20.10 | 0% | 6 ms |
| 192.168.30.10 | 0% | 9 ms |

---

## Cabang A Testing

Pengujian dari Cabang A menuju:

```text
192.168.20.1
192.168.10.10
192.168.30.10
```

Hasil:

| Destination | Packet Loss | Average |
|---|---:|---:|
| 192.168.20.1 | 0% | 0 ms |
| 192.168.10.10 | 0% | 14 ms |
| 192.168.30.10 | 0% | 22 ms |

---

## Cabang B Testing

Pengujian dari Cabang B menuju:

```text
192.168.30.1
192.168.10.10
192.168.20.10
```

Hasil:

| Destination | Packet Loss | Average |
|---|---:|---:|
| 192.168.30.1 | 0% | 0 ms |
| 192.168.10.10 | 0% | 6 ms |
| 192.168.20.10 | 0% | 12 ms |

---

# 📊 Overall Testing Result

Seluruh pengujian yang didokumentasikan menunjukkan:

```text
Packets Sent     : 4
Packets Received : 4
Packet Loss      : 0%
```

Komunikasi berhasil dilakukan antara:

```text
HQ ↔ Cabang A
HQ ↔ Cabang B
Cabang A ↔ Cabang B
```

---

# 🛠️ Troubleshooting

Jika OSPF tidak membentuk adjacency, beberapa pemeriksaan dapat dilakukan.

### 1. Periksa interface

```text
show ip interface brief
```

Pastikan interface WAN berada pada:

```text
up/up
```

### 2. Periksa OSPF neighbor

```text
show ip ospf neighbor
```

Pastikan neighbor berada pada state:

```text
FULL
```

### 3. Periksa routing table

```text
show ip route
```

Pastikan network remote muncul dengan kode:

```text
O
```

### 4. Periksa konfigurasi

```text
show running-config
```

---

# 📂 Repository Structure

```text
dynamic-routing-ospf/
│
├── config/
│   ├── router-HQ.txt
│   ├── router-cabangA.txt
│   ├── router-cabangB.txt
│   ├── ping-PC-HQ.txt
│   ├── ping-PC-cabangA.txt
│   └── ping-PC-cabangB.txt
│
├── topology.png
├── project-5-dynamic-routing-ospf.pkt
└── README.md
```

---

## 🎯 Project Objectives

Project ini bertujuan untuk memahami:

- Dynamic routing
- OSPF
- OSPF neighbor adjacency
- Routing table
- OSPF route
- WAN point-to-point
- Subnetting
- Multi-site network
- Connectivity testing
- Network troubleshooting

---

## 🛠️ Tools & Technologies

- Cisco Packet Tracer
- Cisco Router
- Cisco Switch
- IPv4
- OSPF
- Dynamic Routing
- Serial WAN
- Point-to-Point Network
- Subnetting
- ICMP/Ping
- Cisco IOS CLI

---

## 📚 Learning Outcome

Melalui project ini, saya mempraktikkan implementasi dynamic routing menggunakan OSPF pada jaringan multi-site, termasuk verifikasi OSPF neighbor, pemeriksaan routing table, serta pengujian konektivitas antar jaringan HQ, Cabang A, dan Cabang B.

Project ini juga menjadi latihan untuk memahami perbedaan pendekatan routing statis dan dynamic routing pada jaringan multi-site.
