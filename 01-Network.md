# Network

Catatan networking pribadi untuk belajar serius, praktik command, dan membangun fondasi network yang kuat.

Pembahasan diarahkan ke pemahaman konsep, praktik hands-on, troubleshooting berbasis bukti, dan detail low-level yang sering dibutuhkan saat menganalisis jaringan.

Area utama:

| Area | Cakupan |
|---|---|
| Networking Concepts | OSI/TCP-IP, protocol, port, addressing, topology, cloud, dan traffic type |
| Network Implementation | routing, switching, wireless, cabling, media, dan physical installation |
| Network Operations | monitoring, documentation, change, disaster recovery, dan management access |
| Network Security | segmentation, access control, attack patterns, secure management, dan defense |
| Network Troubleshooting | methodology, packet path, service failure, performance, dan tools |

## Daftar Isi

- [0. Cara Pakai Catatan Ini](#0-cara-pakai-catatan-ini)
- [1.0 Networking Concepts](#10-networking-concepts)
  - [1.1 OSI Model](#11-osi-model)
  - [1.2 Network Appliances, Applications, and Functions](#12-network-appliances-applications-and-functions)
  - [1.3 Cloud Concepts](#13-cloud-concepts)
  - [1.4 Ports, Protocols, Services, and Traffic Types](#14-ports-protocols-services-and-traffic-types)
  - [1.4.1 TCP, UDP, Flags, and Handshake](#141-tcp-udp-flags-and-handshake)
  - [1.5 Transmission Media and Transceivers](#15-transmission-media-and-transceivers)
  - [1.6 Network Topologies, Architectures, and Types](#16-network-topologies-architectures-and-types)
  - [1.7 IPv4 Network Addressing](#17-ipv4-network-addressing)
  - [1.8 Modern Network Environments](#18-modern-network-environments)
  - [1.9 IP Addressing Hands-on Deep Dive](#19-ip-addressing-hands-on-deep-dive)
- [2.0 Network Implementation](#20-network-implementation)
  - [2.1 Routing Technologies](#21-routing-technologies)
  - [2.2 Switching Technologies](#22-switching-technologies)
  - [2.3 Wireless Devices and Technologies](#23-wireless-devices-and-technologies)
  - [2.4 Physical Installations](#24-physical-installations)
- [3.0 Network Operations](#30-network-operations)
  - [3.1 Organizational Processes and Procedures](#31-organizational-processes-and-procedures)
  - [3.2 Network Monitoring](#32-network-monitoring)
  - [3.3 Disaster Recovery](#33-disaster-recovery)
  - [3.4 IPv4 and IPv6 Network Services](#34-ipv4-and-ipv6-network-services)
  - [3.5 Network Access and Management Methods](#35-network-access-and-management-methods)
- [4.0 Network Security](#40-network-security)
  - [4.1 Basic Network Security Concepts](#41-basic-network-security-concepts)
  - [4.2 Network Attacks](#42-network-attacks)
  - [4.3 Security Features, Defense Techniques, and Solutions](#43-security-features-defense-techniques-and-solutions)
- [5.0 Network Troubleshooting](#50-network-troubleshooting)
  - [5.1 Troubleshooting Methodology](#51-troubleshooting-methodology)
  - [5.2 Cabling and Physical Interface Issues](#52-cabling-and-physical-interface-issues)
  - [5.3 Network Service Issues](#53-network-service-issues)
  - [5.4 Performance Issues](#54-performance-issues)
  - [5.5 Tools and Protocols](#55-tools-and-protocols)
- [6.0 Senior Deep Dive](#60-senior-deep-dive)
  - [6.1 Packet Path dan Kernel Networking](#61-packet-path-dan-kernel-networking)
  - [6.2 TCP State, Congestion, dan Socket Tuning](#62-tcp-state-congestion-dan-socket-tuning)
  - [6.3 DNS Delegation, Caching, dan Failure Mode](#63-dns-delegation-caching-dan-failure-mode)
  - [6.4 Routing, ARP/ND, dan Failure Domain](#64-routing-arpnd-dan-failure-domain)
  - [6.5 Observability, Packet Capture, dan Incident Workflow](#65-observability-packet-capture-dan-incident-workflow)
  - [6.6 Performance Engineering dan Capacity Planning](#66-performance-engineering-dan-capacity-planning)

---

## 0. Cara Pakai Catatan Ini

Catatan ini dipakai sebagai baseline konsep sekaligus field notes untuk kerja nyata. Jangan hanya menghafal port atau definisi. Biasakan menjawab:

- masalahnya ada di layer mana
- traffic yang gagal itu source, destination, port, dan protocol apa
- perangkat mana yang seharusnya forwarding, filtering, routing, atau resolving
- output command apa yang membuktikan dugaan
- perubahan mana yang bisa memutus akses
- state apa yang berubah: ARP/ND, route, conntrack, TCP state, DNS cache, atau firewall session
- apakah gejala terjadi di control plane, data plane, atau management plane

Level yang ditargetkan:

| Level | Yang Harus Bisa Dilakukan |
|---|---|
| Administrator | membaca IP, route, DNS, DHCP, firewall, dan service state |
| Network engineer | memahami forwarding path, VLAN, routing, HA, wireless, dan security control |
| Senior/operator | membuktikan root cause dengan packet capture, counters, logs, state table, dan timeline perubahan |

Prinsip catatan senior:

- Selalu pisahkan fakta, dugaan, dan tindakan.
- Jangan percaya satu output command tanpa pembanding dari layer lain.
- Untuk setiap incident, cari `source`, `destination`, `protocol`, `port`, `state`, `path`, dan `last change`.
- Untuk setiap fix, siapkan validasi dan rollback.

Pola belajar di file ini:

- baca konsep singkat, lalu langsung jalankan command yang ada di section tersebut
- sebelum melihat output atau membaca penjelasan lanjutan, prediksi dulu apa yang seharusnya terjadi
- setelah command selesai, tulis observasi: output penting, gejala, layer/komponen yang terlibat, dan dugaan penyebab
- ulangi praktik yang sama di hari berikutnya tanpa melihat catatan agar ingatan dibangun lewat recall
- jalankan command hanya di sistem milik sendiri, lab pribadi, atau environment yang memang kamu diberi izin

---

## 1.0 Networking Concepts

**Praktik setelah bab ini:** buktikan konsep layer, IP, DNS, dan port dengan traffic nyata.

```bash
# Melihat IP address dan interface lokal.
ip addr show

# Melihat default route yang dipakai host.
ip route show

# Query DNS untuk melihat name resolution.
dig example.com

# Melihat HTTP/TLS dari sisi application layer.
curl -v https://example.com/
```

Catat: source IP, destination IP, port, protocol, DNS answer, route yang dipilih, dan layer mana yang sedang kamu buktikan.

Bagian ini membahas fondasi: OSI model, device, cloud, protocol, media, topology, IPv4, dan konsep network modern.

### 1.1 OSI Model

OSI model adalah cara membagi fungsi network menjadi tujuh layer. Model ini membantu troubleshooting karena masalah network bisa diisolasi dari fisik sampai aplikasi.

| Layer | Nama | Fungsi | Contoh |
|---:|---|---|---|
| 7 | Application | layanan yang dipakai aplikasi/user | HTTP, DNS, SMTP |
| 6 | Presentation | encoding, format, encryption | TLS, JPEG, UTF-8 |
| 5 | Session | sesi komunikasi | session control, RPC |
| 4 | Transport | komunikasi end-to-end antar process | TCP, UDP |
| 3 | Network | logical addressing dan routing | IP, ICMP, router |
| 2 | Data Link | frame, MAC address, switching | Ethernet, VLAN, switch |
| 1 | Physical | sinyal fisik | kabel, fiber, radio, transceiver |

PDU per layer:

| Layer | PDU |
|---:|---|
| 7-5 | Data |
| 4 | Segment untuk TCP, datagram untuk UDP |
| 3 | Packet |
| 2 | Frame |
| 1 | Bits |

Detail tiap layer:

| Layer | Nama | PDU | Addressing/Identifier | Device Umum | Contoh Protocol/Teknologi | Yang Dicek Saat Troubleshooting |
|---:|---|---|---|---|---|---|
| 7 | Application | Data | nama aplikasi, URL, query, hostname | proxy, WAF, load balancer L7 | HTTP, HTTPS, DNS, SMTP, IMAP, LDAP, SMB | status aplikasi, response code, DNS answer, auth |
| 6 | Presentation | Data | format/encoding | TLS offloader, proxy | TLS/SSL, certificates, compression, encoding | certificate, cipher, encoding, decryption |
| 5 | Session | Data | session ID/token | gateway, proxy | RPC, NetBIOS session, application session | session timeout, login state, reconnect |
| 4 | Transport | Segment/datagram | TCP/UDP port | firewall, load balancer L4 | TCP, UDP | port open, handshake, retransmission, firewall |
| 3 | Network | Packet | IP address | router, L3 switch, firewall | IPv4, IPv6, ICMP, IPsec | IP, subnet, gateway, route, TTL |
| 2 | Data Link | Frame | MAC address, VLAN ID | switch, bridge, AP | Ethernet, 802.1Q, ARP, STP, Wi-Fi MAC | VLAN, MAC table, ARP, trunk, STP |
| 1 | Physical | Bits | signal, pin, wavelength, frequency | cable, transceiver, repeater | copper, fiber, RF, SFP | link light, speed, duplex, signal, cable |

Penjelasan per layer:

#### Layer 7 - Application

Layer 7 adalah layer yang paling dekat dengan aplikasi. Di sini data sudah punya makna untuk aplikasi, misalnya request HTTP, query DNS, command SMTP, operasi LDAP, atau request SMB.

Layer ini tidak berarti "aplikasi browser" secara literal, tetapi protocol aplikasi yang dipakai browser atau program. Browser memakai HTTP/HTTPS. Mail client memakai SMTP/IMAP. Domain lookup memakai DNS. File sharing Windows memakai SMB.

PDU di Layer 7 biasanya disebut `data` atau `message`, tergantung protocol.

Contoh bentuk HTTP request:

```text
GET /index.html HTTP/1.1
Host: example.com
User-Agent: curl/8.x
Accept: */*
```

Contoh bentuk DNS query secara konseptual:

```text
DNS Query
  Transaction ID: 0x1234
  Flags: standard query
  Question: www.example.com
  Type: A
  Class: IN
```

Yang penting dicek di Layer 7:

- apakah nama domain resolve ke IP yang benar
- apakah HTTP status code benar, misalnya `200`, `301`, `403`, `404`, `500`
- apakah aplikasi menolak karena authentication/authorization
- apakah API path, method, header, atau payload benar
- apakah proxy/WAF/load balancer mengubah request

#### Layer 6 - Presentation

Layer 6 mengatur bagaimana data direpresentasikan agar bisa dipahami kedua sisi. Contohnya encoding, serialization, compression, dan encryption/decryption.

Di network modern, Layer 6 sering terasa saat troubleshooting TLS. Aplikasi bisa benar, port bisa terbuka, tetapi koneksi tetap gagal karena certificate expired, hostname tidak match, cipher suite tidak cocok, atau client tidak trust CA.

PDU Layer 6 juga biasanya disebut `data`, tetapi bentuknya bisa berupa data yang sudah di-encode, compressed, atau encrypted.

Contoh bentuk TLS record secara konseptual:

```text
TLS Record
  Content Type: Handshake / Application Data
  Version: TLS 1.2 or TLS 1.3
  Length: ...
  Encrypted Application Data: ...
```

Contoh masalah Layer 6:

| Gejala | Kemungkinan |
|---|---|
| certificate warning | expired cert, wrong SAN, untrusted CA |
| TLS handshake failed | cipher mismatch, protocol disabled |
| data tidak terbaca | encoding/serialization salah |
| HTTPS bisa dari browser A tapi gagal dari client lama | TLS version/cipher compatibility |

#### Layer 5 - Session

Layer 5 mengatur sesi komunikasi: kapan sesi dibuat, dipertahankan, dipulihkan, atau ditutup. Dalam praktik modern, fungsi session sering digabung ke aplikasi, library, framework, atau protocol tertentu.

Contoh session:

| Contoh | Bentuk Session |
|---|---|
| login web | cookie/session ID/JWT |
| SMB session | authenticated file sharing session |
| RPC session | binding dan context antar process |
| database session | koneksi user ke database |

PDU Layer 5 tetap disebut `data`, tetapi data itu membawa informasi state/session.

Contoh bentuk session cookie di HTTP:

```text
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Lax
```

Yang dicek:

- session timeout
- cookie hilang atau tidak terkirim
- load balancer tidak sticky padahal aplikasi butuh sticky session
- user sudah login tapi server menganggap session invalid
- reconnect gagal setelah network putus

#### Layer 4 - Transport

Layer 4 menghubungkan process ke process memakai port. IP hanya mengantar ke host, sedangkan port menentukan aplikasi/process mana yang menerima data.

TCP bersifat connection-oriented. TCP punya handshake, sequence number, acknowledgment, retransmission, flow control, dan connection state. UDP connectionless; lebih ringan, tetapi reliability ditangani aplikasi jika diperlukan.

PDU:

| Protocol | PDU |
|---|---|
| TCP | segment |
| UDP | datagram |

Contoh bentuk TCP segment:

```text
TCP Segment
  Source Port: 51544
  Destination Port: 443
  Sequence Number: 1000
  Acknowledgment Number: 0
  Flags: SYN
  Window Size: 64240
  Payload: optional application data
```

Contoh bentuk UDP datagram:

```text
UDP Datagram
  Source Port: 53000
  Destination Port: 53
  Length: ...
  Checksum: ...
  Payload: DNS query
```

Yang dicek:

- apakah port tujuan listening
- apakah firewall drop/reject
- apakah TCP handshake selesai
- apakah ada retransmission atau reset
- apakah UDP response kembali
- apakah NAT mengubah port/source

#### Layer 3 - Network

Layer 3 mengatur logical addressing dan routing. Di sinilah IPv4/IPv6 bekerja. Router membaca destination IP dan menentukan next hop.

PDU Layer 3 disebut `packet`.

Contoh bentuk IPv4 packet:

```text
IPv4 Packet
  Version: 4
  Source IP: 192.168.1.10
  Destination IP: 10.10.10.50
  TTL: 64
  Protocol: TCP
  Payload: TCP segment
```

Contoh bentuk IPv6 packet:

```text
IPv6 Packet
  Version: 6
  Source IP: 2001:db8::10
  Destination IP: 2001:db8::50
  Hop Limit: 64
  Next Header: TCP
  Payload: TCP segment
```

Hal penting:

- IP source/destination biasanya tetap end-to-end
- NAT bisa mengubah IP source/destination
- TTL/hop limit berkurang setiap melewati router
- routing memilih next hop
- subnet mask/prefix menentukan apakah tujuan local atau remote

Yang dicek:

- IP address dan prefix
- default gateway
- route table
- ICMP reachability
- asymmetric routing
- NAT
- ACL/firewall Layer 3

#### Layer 2 - Data Link

Layer 2 bekerja dalam satu broadcast domain atau satu link lokal. Ethernet frame memakai MAC address, bukan IP address. Switch mengambil keputusan forwarding berdasarkan MAC address table.

PDU Layer 2 disebut `frame`.

Contoh bentuk Ethernet frame:

```text
Ethernet Frame
  Preamble: ...
  Destination MAC: bb:bb:bb:bb:bb:bb
  Source MAC: aa:aa:aa:aa:aa:aa
  802.1Q VLAN Tag: optional
  EtherType: IPv4 / IPv6 / ARP
  Payload: IP packet or ARP message
  FCS: frame check sequence
```

Jika VLAN tagging aktif, frame membawa tag 802.1Q:

```text
802.1Q Tag
  TPID: 0x8100
  PCP: priority
  DEI: drop eligible
  VLAN ID: 10
```

ARP juga berada di sekitar Layer 2/3 karena tugasnya memetakan IPv4 ke MAC address dalam local network.

Contoh ARP:

```text
ARP Request
  Who has 192.168.1.20?
  Tell 192.168.1.10

ARP Reply
  192.168.1.20 is at bb:bb:bb:bb:bb:bb
```

Yang dicek:

- VLAN access/trunk/native VLAN
- MAC address table
- ARP table
- STP blocking/loop
- port security
- duplex mismatch
- Wi-Fi association

#### Layer 1 - Physical

Layer 1 adalah sinyal fisik. Di sini belum ada IP, port, HTTP, atau DNS. Yang ada adalah bit yang dikirim sebagai electrical signal, light pulse, atau radio wave.

PDU Layer 1 disebut `bits`.

Contoh bentuk konseptual:

```text
Bits
  01001000 01010100 01010100 01010000 ...

Media
  copper electrical signal
  fiber light pulse
  wireless radio frequency
```

Yang dicek:

- kabel putus atau salah tipe
- transceiver/SFP tidak cocok
- light level fiber
- Wi-Fi signal/RSSI/SNR
- speed dan duplex negotiation
- CRC/error counter
- link light
- power/PoE

Ringkasan bentuk PDU:

```text
Layer 7 Application   : HTTP request, DNS message, SMTP command
Layer 6 Presentation  : encoded/compressed/encrypted data, TLS record
Layer 5 Session       : session data, cookie, RPC context
Layer 4 Transport     : TCP segment or UDP datagram
Layer 3 Network       : IP packet
Layer 2 Data Link     : Ethernet frame
Layer 1 Physical      : bits as signal
```

Cara data dibungkus saat dikirim:

```text
Application data
-> TCP segment / UDP datagram
-> IP packet
-> Ethernet frame
-> Bits on wire/radio/fiber
```

Encapsulation:

| Tahap | Yang Ditambahkan |
|---|---|
| Layer 4 | source port, destination port, TCP/UDP header |
| Layer 3 | source IP, destination IP, TTL/hop limit |
| Layer 2 | source MAC, destination MAC, VLAN tag jika ada |
| Layer 1 | sinyal fisik |

Decapsulation adalah proses sebaliknya saat data diterima.

Contoh frame/packet/segment:

```text
Ethernet frame
  Source MAC: aa:aa:aa:aa:aa:aa
  Destination MAC: bb:bb:bb:bb:bb:bb
  EtherType: IPv4

IP packet
  Source IP: 192.168.1.10
  Destination IP: 93.184.216.34
  Protocol: TCP

TCP segment
  Source port: 51544
  Destination port: 443
  Flags: SYN/ACK/FIN/RST

Application data
  HTTP request / TLS encrypted data / DNS query
```

Contoh alur HTTP:

1. Browser membuat request HTTP.
2. TLS mengenkripsi data jika memakai HTTPS.
3. TCP membuat koneksi ke port 443.
4. IP menentukan alamat tujuan.
5. Ethernet membungkus packet menjadi frame menuju next hop.
6. Physical layer mengirim sinyal lewat kabel/fiber/wireless.

Cara troubleshooting dengan OSI:

- Layer 1: cek link, kabel, transceiver, power, radio signal.
- Layer 2: cek MAC address, VLAN, trunk, STP, switch port.
- Layer 3: cek IP, subnet mask, gateway, route.
- Layer 4: cek port TCP/UDP, firewall, connection state.
- Layer 7: cek DNS, HTTP response, authentication, aplikasi.

Contoh mapping masalah:

| Gejala | Layer Kemungkinan | Dugaan |
|---|---:|---|
| Link down | 1 | kabel, transceiver, power, signal |
| VLAN salah | 2 | access/trunk/native VLAN |
| Bisa ping gateway tapi tidak internet | 3 | default route, NAT, upstream |
| Ping bisa tapi HTTPS gagal | 4/7 | firewall port 443, service down, TLS/app |
| DNS gagal tapi akses IP bisa | 7 | resolver, record, DNS filtering |
| Certificate warning | 6/7 | expired cert, wrong hostname, untrusted CA |

Packet walk: host ke host satu subnet.

Contoh:

```text
Host A: 192.168.1.10/24
Host B: 192.168.1.20/24
Gateway: 192.168.1.1
```

Alur:

1. Host A menghitung bahwa `192.168.1.20` masih satu subnet.
2. Host A butuh MAC address Host B.
3. Host A mengirim ARP request broadcast: "Who has 192.168.1.20?"
4. Host B membalas ARP reply dengan MAC address miliknya.
5. Host A mengirim Ethernet frame langsung ke MAC Host B.

Header yang penting:

| Header | Source | Destination |
|---|---|---|
| Ethernet | MAC Host A | MAC Host B |
| IP | `192.168.1.10` | `192.168.1.20` |

Packet walk: host ke host beda subnet.

Contoh:

```text
Host A: 192.168.1.10/24
Server: 10.10.10.50/24
Gateway Host A: 192.168.1.1
```

Alur:

1. Host A menghitung bahwa `10.10.10.50` bukan satu subnet.
2. Host A memilih default gateway `192.168.1.1`.
3. Host A mencari MAC gateway lewat ARP.
4. Host A mengirim frame ke MAC gateway, tetapi destination IP tetap `10.10.10.50`.
5. Router mengganti Layer 2 header untuk next hop berikutnya.
6. Source/destination IP tetap sama, kecuali ada NAT.

Header pada hop pertama:

| Header | Source | Destination |
|---|---|---|
| Ethernet | MAC Host A | MAC Gateway |
| IP | `192.168.1.10` | `10.10.10.50` |

Hal penting:

- MAC address berubah setiap hop Layer 3.
- IP address tetap end-to-end kecuali NAT.
- TTL/hop limit berkurang setiap melewati router.
- Switch melihat MAC address.
- Router melihat IP address.

### 1.2 Network Appliances, Applications, and Functions

Perangkat network punya fungsi berbeda. Dalam troubleshooting, penting tahu perangkat mana yang membuat keputusan forwarding, filtering, inspection, atau translation.

| Perangkat/Fungsi | Fungsi Utama | Catatan |
|---|---|---|
| Router | menghubungkan network berbeda | bekerja dominan di Layer 3 |
| Switch | menghubungkan host dalam LAN | bekerja dominan di Layer 2 |
| Firewall | mengizinkan/menolak traffic | bisa L3/L4/L7 |
| IDS | mendeteksi traffic mencurigakan | biasanya passive/monitoring |
| IPS | mendeteksi dan memblokir traffic | inline, bisa drop packet |
| Load balancer | membagi traffic ke backend | L4 atau L7 |
| Proxy | menjadi perantara request client | forward atau reverse proxy |
| VPN concentrator | terminasi tunnel VPN | remote access/site-to-site |
| Access point | bridge wireless ke wired LAN | 802.11 |
| Modem/ONT | terminasi koneksi ISP | cable, DSL, fiber |
| Hub | mengulang sinyal ke semua port | legacy, collision domain besar |
| Bridge | menghubungkan segment Layer 2 | konsep lama yang mirip fungsi switch |
| Wireless controller | mengelola banyak AP | on-prem atau cloud-managed |
| Media converter | mengubah copper ke fiber atau sebaliknya | tidak melakukan routing |
| VoIP gateway | menghubungkan voice IP ke PSTN/telephony | SIP/RTP |
| Content filter | memblokir kategori/domain/content | sering di proxy/firewall |
| NAS | file storage lewat network | SMB/NFS, file-level access |
| SAN | block storage network | Fibre Channel/iSCSI, block-level access |

Application dan function penting:

| Item | Arti | Catatan Operasional |
|---|---|---|
| CDN | cache/distribusi content dekat user | mengurangi latency dan beban origin |
| VPN | tunnel terenkripsi antar user/site/network | remote access atau site-to-site |
| QoS | klasifikasi dan prioritas traffic | penting untuk voice/video dan link congested |
| TTL | hop limit IPv4 packet | turun 1 setiap melewati router |

NAS vs SAN:

| Item | NAS | SAN |
|---|---|---|
| Access | file-level | block-level |
| Protocol umum | SMB, NFS | Fibre Channel, iSCSI |
| Contoh use case | file share user/team | storage VM/database |
| Terlihat oleh client | folder/file share | disk/block device |

Firewall vs IDS vs IPS:

| Teknologi | Bisa Melihat | Bisa Memblokir | Contoh Use Case |
|---|---|---|---|
| Firewall | header dan kadang aplikasi | ya | block port/source/destination |
| IDS | traffic mirror/copy | tidak langsung | alert serangan |
| IPS | traffic inline | ya | drop exploit traffic |

Load balancer:

| Mode | Keputusan Berdasarkan | Contoh |
|---|---|---|
| Layer 4 | IP dan port | TCP 443 ke backend |
| Layer 7 | HTTP header/path/cookie | `/api` ke backend API |

Reverse proxy vs forward proxy:

| Proxy | Siapa yang Dilindungi/Diwakili | Contoh |
|---|---|---|
| Forward proxy | client internal | user kantor akses internet lewat proxy |
| Reverse proxy | server/backend | Nginx/HAProxy menerima traffic publik lalu meneruskan ke app |

Load balancer field umum:

```text
VIP: 203.0.113.10:443
Pool: web-backend
Members:
  10.0.1.11:443
  10.0.1.12:443
Health check: HTTPS /health
Algorithm: round-robin
```

Field:

| Field | Arti |
|---|---|
| VIP | IP/port virtual yang diakses client |
| Pool | kumpulan backend server |
| Member | server real di belakang load balancer |
| Health check | tes untuk menentukan backend sehat |
| Algorithm | cara memilih backend |

Algorithm umum:

| Algorithm | Cara Kerja |
|---|---|
| Round-robin | bergantian |
| Least connections | pilih backend dengan koneksi paling sedikit |
| Source IP hash | client yang sama diarahkan ke backend yang sama |

WAF:

- WAF adalah firewall Layer 7 untuk aplikasi web.
- Bisa memblokir pola serangan seperti SQL injection, XSS, path traversal.
- WAF tidak menggantikan secure coding, patching, atau authentication yang benar.

Network appliance placement:

| Perangkat | Umum Ditempatkan Di | Alasan |
|---|---|---|
| Firewall edge | antara internet dan internal/DMZ | kontrol traffic masuk/keluar |
| IDS sensor | SPAN/TAP dekat traffic penting | melihat traffic tanpa inline risk |
| IPS | inline sebelum aset penting | bisa drop traffic berbahaya |
| WAF | depan aplikasi web | inspeksi HTTP/HTTPS |
| Load balancer | depan pool server | availability dan distribusi traffic |
| VPN concentrator | edge atau DMZ | terminasi remote/site tunnel |

Stateful firewall table:

```text
Source              Destination         State
10.10.10.55:51544   203.0.113.10:443    ESTABLISHED
```

Field:

| Field | Arti |
|---|---|
| Source | IP/port asal koneksi |
| Destination | IP/port tujuan |
| State | status koneksi yang dilacak firewall |

Stateful firewall mengizinkan return traffic berdasarkan state table. Karena itu asymmetric routing bisa membuat return traffic terlihat "tidak punya state" dan akhirnya diblok.

### 1.3 Cloud Concepts

Cloud networking mengubah lokasi perangkat, tetapi konsep network tetap sama: subnet, routing, firewall/security group, NAT, load balancer, VPN, dan DNS.

Network functions virtualization:

- NFV memindahkan fungsi network dari appliance fisik ke software/virtual appliance.
- Contoh: virtual firewall, virtual router, virtual load balancer, virtual WAN edge.
- Manfaatnya provisioning lebih cepat, scale lebih fleksibel, dan cocok untuk cloud/virtualized data center.
- Risikonya performa bergantung pada host, hypervisor, NIC offload, dan resource isolation.

Service model:

| Model | Yang Dikelola Provider | Yang Dikelola Customer |
|---|---|---|
| IaaS | compute, storage, network fisik | OS, aplikasi, konfigurasi network virtual |
| PaaS | platform runtime | aplikasi dan data |
| SaaS | aplikasi penuh | konfigurasi user/data |

Deployment model:

| Model | Arti |
|---|---|
| Public cloud | resource di provider cloud |
| Private cloud | cloud untuk satu organisasi |
| Hybrid cloud | gabungan private dan public |
| Multicloud | memakai lebih dari satu provider cloud |

Konsep penting:

- VPC/VNet adalah network virtual di cloud.
- Subnet membagi address space.
- Security group/firewall rule mengontrol traffic.
- Route table menentukan next hop.
- NAT gateway memberi outbound internet untuk private subnet.
- Load balancer menerima traffic client dan meneruskan ke backend.

Cloud connectivity:

| Opsi | Arti | Catatan |
|---|---|---|
| Internet gateway | akses public internet untuk subnet public | butuh public IP/route |
| NAT gateway | outbound internet dari private subnet | inbound langsung tidak dibuka |
| Site-to-site VPN | tunnel dari kantor ke cloud | cepat dibuat, bergantung internet |
| Dedicated private link / Direct Connect | koneksi privat ke cloud provider | lebih stabil, lebih mahal |
| VPC/VNet peering | menghubungkan dua network cloud | routing antar VPC/VNet |
| Transit gateway/hub | hub routing banyak VPC/site | memudahkan skala besar |

North-south vs east-west:

| Traffic | Arti | Contoh |
|---|---|---|
| North-south | traffic masuk/keluar data center/cloud | user internet ke load balancer |
| East-west | traffic antar workload internal | app server ke database |

Shared responsibility:

| Layer | Umumnya Provider | Umumnya Customer |
|---|---|---|
| Physical facility | ya | tidak |
| Physical network | ya | tidak |
| Virtual network config | sebagian | subnet, route, security group/firewall |
| Operating system | tergantung model | ya pada IaaS |
| Application/data | tergantung model | biasanya customer |

Contoh VPC sederhana:

```text
VPC: 10.0.0.0/16

Public subnet:  10.0.1.0/24
Private subnet: 10.0.2.0/24

Internet Gateway: attached to VPC
NAT Gateway: in public subnet
Load Balancer: public subnet
App Server: private subnet
Database: private subnet
```

Route table public subnet:

| Destination | Target | Arti |
|---|---|---|
| `10.0.0.0/16` | local | traffic internal VPC |
| `0.0.0.0/0` | Internet Gateway | outbound/inbound internet route |

Route table private subnet:

| Destination | Target | Arti |
|---|---|---|
| `10.0.0.0/16` | local | traffic internal VPC |
| `0.0.0.0/0` | NAT Gateway | outbound internet tanpa inbound langsung |

Security group rule:

| Direction | Source | Port | Arti |
|---|---|---:|---|
| Inbound | `0.0.0.0/0` | 443 | publik boleh akses HTTPS load balancer |
| Inbound | security group load balancer | 443 | hanya load balancer boleh akses app |
| Inbound | security group app | 5432 | hanya app boleh akses database |

Network ACL vs security group:

| Fitur | Security Group | Network ACL |
|---|---|---|
| Scope | interface/instance | subnet |
| State | stateful | stateless |
| Rule | allow rules | allow dan deny rules |
| Evaluasi | semua rule relevan | berdasarkan urutan rule |

Scalability vs elasticity:

| Konsep | Arti |
|---|---|
| Scalability | kemampuan sistem diperbesar untuk menangani beban |
| Elasticity | kemampuan scale up/down otomatis mengikuti demand |
| Multitenancy | banyak customer berbagi platform fisik/logis dengan isolasi |

### 1.4 Ports, Protocols, Services, and Traffic Types

Port membantu membedakan service pada host yang sama. Protocol menentukan aturan komunikasi.

Port penting:

| Protocol/Service | Port | Transport | Fungsi |
|---|---:|---|---|
| FTP data/control | 20/21 | TCP | file transfer lama |
| SFTP/SSH | 22 | TCP | remote shell dan transfer aman |
| Telnet | 23 | TCP | remote shell tidak terenkripsi |
| SMTP | 25 | TCP | mail transfer |
| TACACS+ | 49 | TCP | AAA device administration |
| DNS | 53 | UDP/TCP | name resolution |
| DHCP | 67/68 | UDP | dynamic IP assignment |
| TFTP | 69 | UDP | file transfer sederhana |
| HTTP | 80 | TCP | web tidak terenkripsi |
| Kerberos | 88 | TCP/UDP | authentication |
| POP3 | 110 | TCP | mail retrieval |
| NTP | 123 | UDP | time sync |
| NetBIOS | 137-139 | TCP/UDP | legacy Windows name/session |
| IMAP | 143 | TCP | mailbox access |
| SNMP | 161/162 | UDP | monitoring dan trap |
| BGP | 179 | TCP | routing antar autonomous system |
| LDAP | 389 | TCP/UDP | directory service |
| HTTPS | 443 | TCP/UDP | web terenkripsi; UDP umum untuk HTTP/3/QUIC |
| SMB | 445 | TCP | file sharing |
| SMTPS/submissions | 465 | TCP | mail submission dengan implicit TLS |
| Syslog | 514 | UDP/TCP | log forwarding |
| SMTP submission | 587 | TCP | mail submission, biasanya STARTTLS |
| LDAPS | 636 | TCP | LDAP over TLS |
| SQL Server | 1433 | TCP | Microsoft SQL Server |
| RADIUS | 1812/1813 | UDP | AAA authentication/accounting |
| NFS | 2049 | TCP/UDP | network file system |
| RDP | 3389 | TCP/UDP | remote desktop |
| RTP/SRTP | dynamic, sering 5004/5005 | UDP | voice/video media |
| SIP/SIPS | 5060, 5061 | UDP/TCP, TCP/TLS | VoIP signaling |

Protocol category:

| Category | Protocol |
|---|---|
| File transfer | FTP, SFTP, TFTP, SMB, NFS |
| Remote access | SSH, RDP, Telnet |
| Email | SMTP, POP3, IMAP |
| Name/address/time | DNS, DHCP, NTP |
| Directory/auth | LDAP, LDAPS, Kerberos, RADIUS, TACACS+ |
| Monitoring/logging | SNMP, Syslog |
| Web | HTTP, HTTPS |
| Voice/video | SIP, RTP/SRTP |
| Routing | BGP |

IP protocol types:

| Protocol | Layer | Arti | Catatan |
|---|---:|---|---|
| ICMP | 3 | control/error message untuk IP | dipakai `ping`, unreachable, TTL exceeded |
| TCP | 4 | reliable byte stream | handshake, stateful |
| UDP | 4 | connectionless datagram | low overhead |
| GRE | 3/overlay | tunneling sederhana | tidak terenkripsi sendiri |
| IPsec AH | 3 | authentication/integrity untuk IP packet | tidak umum dipakai dibanding ESP |
| IPsec ESP | 3 | encryption/integrity payload | umum untuk VPN |
| IKE | control plane | negosiasi IPsec SA/key | UDP 500/4500 pada banyak implementasi |

IPsec flow ringkas:

```text
IKE negotiation
-> Security Association terbentuk
-> ESP/AH melindungi traffic
-> tunnel atau transport mode membawa packet
```

### 1.4.1 TCP, UDP, Flags, and Handshake

TCP dan UDP sama-sama berada di Layer 4, tetapi cara kerjanya berbeda. TCP membuat koneksi dan menjaga urutan data. UDP tidak membuat koneksi, sehingga lebih ringan tetapi tidak memberi jaminan delivery dari protocol itu sendiri.

| Protocol | Karakter | Cocok Untuk |
|---|---|---|
| TCP | connection-oriented, reliable, ordered | web, SSH, email, file transfer |
| UDP | connectionless, low overhead | DNS, DHCP, VoIP, streaming, gaming |

TCP flags:

| Flag | Arti |
|---|---|
| SYN | mulai koneksi |
| ACK | acknowledgement |
| FIN | selesai normal |
| RST | reset koneksi |
| PSH | meminta data segera dikirim ke aplikasi |
| URG | urgent pointer valid, jarang dipakai modern |

#### TCP Three-Way Handshake

TCP harus membuat koneksi sebelum data aplikasi dikirim. Proses ini disebut three-way handshake.

Contoh client `192.168.1.10` membuka HTTPS ke server `93.184.216.34:443`:

```text
1. Client -> Server: SYN
   Source IP: 192.168.1.10
   Source Port: 51544
   Destination IP: 93.184.216.34
   Destination Port: 443
   Flag: SYN

2. Server -> Client: SYN, ACK
   Source IP: 93.184.216.34
   Source Port: 443
   Destination IP: 192.168.1.10
   Destination Port: 51544
   Flags: SYN, ACK

3. Client -> Server: ACK
   Source IP: 192.168.1.10
   Source Port: 51544
   Destination IP: 93.184.216.34
   Destination Port: 443
   Flag: ACK
```

Tabel handshake:

| Step | Arah | Flags | Arti |
|---:|---|---|---|
| 1 | Client ke server | SYN | client meminta koneksi |
| 2 | Server ke client | SYN-ACK | server menerima dan membalas request koneksi |
| 3 | Client ke server | ACK | client mengonfirmasi, koneksi established |

Setelah handshake:

- TCP connection dianggap established.
- Data aplikasi bisa mulai dikirim.
- Untuk HTTPS, TLS handshake terjadi setelah TCP handshake.
- Untuk HTTP biasa, request HTTP bisa dikirim setelah TCP established.

Urutan HTTPS sederhana:

```text
DNS lookup
-> TCP three-way handshake ke port 443
-> TLS handshake
-> HTTP request terenkripsi
-> HTTP response terenkripsi
-> TCP connection close atau keep-alive
```

TCP teardown:

```text
Client -> Server: FIN
Server -> Client: ACK
Server -> Client: FIN
Client -> Server: ACK
```

Tabel teardown:

| Step | Flags | Arti |
|---:|---|---|
| 1 | FIN | pihak pertama ingin menutup koneksi |
| 2 | ACK | pihak kedua mengakui FIN |
| 3 | FIN | pihak kedua juga menutup sisi koneksinya |
| 4 | ACK | koneksi selesai ditutup |

RST:

- `RST` memutus koneksi secara paksa.
- Sering terlihat jika port tertutup, service menolak koneksi, firewall melakukan reset, atau aplikasi crash.

TCP sequence dan acknowledgement:

| Field | Arti |
|---|---|
| Sequence number | nomor byte pertama dalam segment |
| Acknowledgement number | byte berikutnya yang diharapkan penerima |
| Window size | jumlah data yang bisa diterima sebelum ACK berikutnya |

TCP header field umum:

| Field | Arti |
|---|---|
| Source port | port asal |
| Destination port | port tujuan |
| Sequence number | posisi byte dalam stream |
| Acknowledgement number | byte berikutnya yang diharapkan |
| Flags | SYN, ACK, FIN, RST, dan lainnya |
| Window size | receive window |
| Checksum | validasi error header/data |

UDP header field:

| Field | Arti |
|---|---|
| Source port | port asal |
| Destination port | port tujuan |
| Length | panjang UDP datagram |
| Checksum | validasi error |

MTU dan MSS:

| Konsep | Arti |
|---|---|
| MTU | ukuran maksimum packet/frame yang bisa lewat link |
| MSS | ukuran maksimum TCP payload dalam segment |
| Fragmentation | packet dipecah karena terlalu besar |
| PMTUD | path MTU discovery untuk menemukan ukuran aman |

Gejala TCP umum:

| Gejala di capture | Kemungkinan |
|---|---|
| SYN berulang tanpa SYN-ACK | server tidak reachable, firewall drop, routing issue |
| SYN lalu RST | port tertutup atau firewall reject |
| Banyak retransmission | packet loss, congestion, bad link |
| Zero window | penerima tidak sanggup menerima data |
| Handshake sukses tapi aplikasi gagal | Layer 7/TLS/auth/service issue |

UDP tidak punya handshake:

- UDP langsung mengirim datagram.
- Tidak ada koneksi established.
- Reliability harus ditangani aplikasi jika dibutuhkan.
- Cocok untuk DNS, DHCP, VoIP, streaming, dan traffic yang sensitif latency.

Socket:

```text
192.168.1.10:51544 -> 93.184.216.34:443 TCP
```

Field socket:

| Field | Contoh | Arti |
|---|---|---|
| source IP | `192.168.1.10` | host asal |
| source port | `51544` | ephemeral port client |
| destination IP | `93.184.216.34` | host tujuan |
| destination port | `443` | service tujuan |
| protocol | `TCP` | transport protocol |

Ephemeral port:

- Port sementara yang dipilih client untuk koneksi keluar.
- Server membalas ke ephemeral port tersebut.
- Banyak koneksi ke server yang sama bisa dibedakan dari kombinasi source IP, source port, destination IP, destination port, dan protocol.

DNS query transport:

| Kondisi | Transport |
|---|---|
| query kecil biasa | UDP 53 |
| response besar / zone transfer | TCP 53 |
| DNS over TLS | TCP 853 |
| DNS over HTTPS | HTTPS 443 |

Traffic type:

| Type | Arti | Contoh |
|---|---|---|
| Unicast | satu ke satu | client ke web server |
| Broadcast | satu ke semua dalam broadcast domain | ARP request |
| Multicast | satu ke group | video distribution |
| Anycast | satu ke node terdekat dari banyak node | public DNS |

### 1.5 Transmission Media and Transceivers

Media menentukan bagaimana data dikirim secara fisik: copper, fiber, atau wireless.

Copper cable:

| Tipe | Catatan |
|---|---|
| Cat 5e | umum untuk 1 Gbps |
| Cat 6 | 1 Gbps, bisa 10 Gbps jarak terbatas |
| Cat 6a | 10 Gbps lebih stabil |
| Cat 7/8 | shielding lebih tinggi, data center/special use |
| UTP | unshielded twisted pair |
| STP | shielded twisted pair |
| Coaxial | kabel koaksial, umum pada cable internet/CCTV lama |
| Twinaxial | kabel pendek high-speed, sering pada DAC data center |

802.3 Ethernet standards:

| Standard | Media Umum | Speed |
|---|---|---:|
| 100BASE-TX | copper twisted pair | 100 Mbps |
| 1000BASE-T | copper twisted pair | 1 Gbps |
| 10GBASE-T | copper twisted pair | 10 Gbps |
| 1000BASE-SX/LX | fiber | 1 Gbps |
| 10GBASE-SR/LR | fiber | 10 Gbps |
| 40GBASE-SR4/LR4 | fiber | 40 Gbps |
| 100GBASE-SR4/LR4 | fiber | 100 Gbps |

Fiber:

| Tipe | Arti | Catatan |
|---|---|---|
| Single-mode | core kecil, jarak jauh | biasanya laser |
| Multimode | core lebih besar, jarak lebih pendek | data center/campus |

Connector:

| Connector | Umum Untuk |
|---|---|
| RJ45 | copper Ethernet |
| LC | fiber modern, high density |
| SC | fiber lama/telecom |
| ST | fiber lama |
| MPO/MTP | high-density fiber |
| RJ11 | telephony/DSL lama |
| F-type | coaxial cable TV/cable modem |
| BNC | coaxial, lab/CCTV/legacy network |

Transceiver:

| Tipe | Catatan |
|---|---|
| SFP | 1 Gbps |
| SFP+ | 10 Gbps |
| QSFP | 40/100 Gbps class |
| DAC | direct attach copper, jarak pendek |
| AOC | active optical cable |

Transceiver protocol:

| Protocol | Arti |
|---|---|
| Ethernet | umum untuk LAN/data center IP network |
| Fibre Channel | umum untuk SAN storage |

Wireless non-Wi-Fi media:

| Media | Arti | Contoh |
|---|---|---|
| Cellular | koneksi mobile carrier | LTE/5G backup WAN |
| Satellite | koneksi via satelit | remote site, maritime, disaster recovery |

Ethernet copper pinout:

| Standard | Pair Order |
|---|---|
| T568A | white/green, green, white/orange, blue, white/blue, orange, white/brown, brown |
| T568B | white/orange, orange, white/green, blue, white/blue, green, white/brown, brown |

Straight-through vs crossover:

| Cable | Ujung A | Ujung B | Catatan |
|---|---|---|---|
| Straight-through | T568A | T568A | umum |
| Straight-through | T568B | T568B | paling umum |
| Crossover | T568A | T568B | device lama sejenis, sekarang sering digantikan Auto-MDI/MDIX |

Fiber detail:

| Item | Single-mode | Multimode |
|---|---|---|
| Core | kecil | lebih besar |
| Jarak | jauh | pendek/sedang |
| Light source | laser | LED/VCSEL |
| Warna umum | kuning | orange/aqua |

Transceiver compatibility:

- Speed harus cocok dengan port/perangkat.
- Fiber type harus cocok: single-mode dengan optic single-mode, multimode dengan optic multimode.
- Wavelength harus cocok.
- Connector harus cocok atau memakai patch cord yang sesuai.
- Vendor lock bisa membuat optic tidak diterima perangkat tertentu.

Karakteristik sinyal:

- Attenuation: sinyal melemah karena jarak.
- Interference: gangguan dari sumber eksternal.
- Crosstalk: gangguan antar kabel/pair.
- Duplex: half/full-duplex.
- Speed: 100 Mbps, 1 Gbps, 10 Gbps, dan seterusnya.

### 1.6 Network Topologies, Architectures, and Types

Topology menjelaskan bentuk hubungan antar perangkat.

Topology:

| Topology | Arti | Catatan |
|---|---|---|
| Star | semua node ke central switch | umum di LAN |
| Mesh | banyak jalur antar node | redundancy tinggi |
| Hybrid | gabungan beberapa topology | paling umum di real world |
| Point-to-point | dua perangkat langsung | WAN link, wireless bridge |
| Hub-and-spoke | branch ke hub pusat | WAN tradisional |

Network type:

| Type | Arti |
|---|---|
| LAN | local area network |
| WLAN | wireless LAN |
| WAN | wide area network |
| MAN | metropolitan area network |
| SAN | storage area network |
| PAN | personal area network |
| CAN | campus area network |

Architecture:

| Architecture | Arti |
|---|---|
| Three-tier | access, distribution, core |
| Collapsed core | core dan distribution digabung | cocok network kecil/menengah |
| Spine-leaf | data center fabric modern |
| SD-WAN | WAN berbasis policy dan overlay |
| Client-server | client memakai layanan server |
| Peer-to-peer | node langsung saling melayani |
| On-premises | infrastructure dikelola di lokasi organisasi |
| Colocation | hardware milik organisasi ditempatkan di data center pihak ketiga |
| Branch office | site cabang yang terhubung ke HQ/cloud |

Three-tier detail:

| Layer | Fungsi |
|---|---|
| Access | tempat end device terhubung |
| Distribution | policy, routing antar VLAN, aggregation |
| Core | backbone cepat dan stabil |

Spine-leaf detail:

| Komponen | Fungsi |
|---|---|
| Leaf | tempat server/device terhubung |
| Spine | menghubungkan semua leaf |

Kenapa spine-leaf dipakai:

- jalur antar server lebih konsisten
- cocok untuk east-west traffic data center
- mudah scale dengan menambah spine/leaf
- mengurangi bottleneck tradisional

### 1.7 IPv4 Network Addressing

IPv4 address panjangnya 32-bit, ditulis sebagai empat octet.

Contoh:

```text
192.168.10.25/24
```

Field:

| Bagian | Nilai | Arti |
|---|---|---|
| IP address | `192.168.10.25` | alamat host |
| Prefix | `/24` | 24 bit network |
| Subnet mask | `255.255.255.0` | mask setara `/24` |
| Network ID | `192.168.10.0` | alamat network |
| Broadcast | `192.168.10.255` | alamat broadcast subnet |
| Host range | `.1` sampai `.254` | host usable |

Private IPv4:

| Range | CIDR | Penggunaan |
|---|---|---|
| 10.0.0.0 - 10.255.255.255 | `10.0.0.0/8` | private |
| 172.16.0.0 - 172.31.255.255 | `172.16.0.0/12` | private |
| 192.168.0.0 - 192.168.255.255 | `192.168.0.0/16` | private |
| 169.254.0.0 - 169.254.255.255 | `169.254.0.0/16` | APIPA/link-local |
| 127.0.0.0 - 127.255.255.255 | `127.0.0.0/8` | loopback |

Classful IPv4 lama:

| Class | Range Awal | Default Mask | Catatan |
|---|---|---|---|
| A | `1.0.0.0 - 126.255.255.255` | `/8` | unicast lama, `127/8` loopback |
| B | `128.0.0.0 - 191.255.255.255` | `/16` | unicast lama |
| C | `192.0.0.0 - 223.255.255.255` | `/24` | unicast lama |
| D | `224.0.0.0 - 239.255.255.255` | n/a | multicast |
| E | `240.0.0.0 - 255.255.255.255` | n/a | reserved/experimental |

Catatan:

- Desain modern memakai CIDR, bukan classful routing.
- Class A/B/C masih sering muncul di materi dasar, legacy system, dan percakapan troubleshooting.

Common prefix:

| Prefix | Mask | Usable Host |
|---:|---|---:|
| `/24` | `255.255.255.0` | 254 |
| `/25` | `255.255.255.128` | 126 |
| `/26` | `255.255.255.192` | 62 |
| `/27` | `255.255.255.224` | 30 |
| `/28` | `255.255.255.240` | 14 |
| `/29` | `255.255.255.248` | 6 |
| `/30` | `255.255.255.252` | 2 |
| `/32` | `255.255.255.255` | 1 host route |

Subnetting cepat:

1. Tentukan prefix.
2. Cari block size pada octet yang berubah.
3. Tentukan network ID.
4. Tentukan broadcast.
5. Tentukan host range.

Contoh `/26`:

```text
192.168.1.130/26
```

Analisis:

| Item | Nilai |
|---|---|
| Mask | `255.255.255.192` |
| Block size | `64` |
| Network ID | `192.168.1.128` |
| Broadcast | `192.168.1.191` |
| Usable host | `192.168.1.129 - 192.168.1.190` |

VLSM:

- VLSM berarti memakai subnet mask berbeda-beda dalam satu address space.
- Tujuannya mengurangi pemborosan IP.
- Subnet terbesar sebaiknya dialokasikan lebih dulu.

Contoh kebutuhan:

| Segment | Kebutuhan Host | Prefix Cocok | Usable |
|---|---:|---:|---:|
| User LAN | 100 | `/25` | 126 |
| Server LAN | 50 | `/26` | 62 |
| Printer | 20 | `/27` | 30 |
| Point-to-point | 2 | `/30` | 2 |

CIDR route summarization:

```text
10.10.0.0/24
10.10.1.0/24
10.10.2.0/24
10.10.3.0/24
```

Bisa diringkas menjadi:

```text
10.10.0.0/22
```

Tujuannya:

- routing table lebih kecil
- route advertisement lebih ringkas
- policy routing lebih mudah dikelola

### 1.8 Modern Network Environments

Modern network banyak memakai overlay, automation, identity-aware access, dan policy central.

| Konsep | Arti |
|---|---|
| SDN | control plane dipisah dan dikelola software |
| SD-WAN | WAN overlay berbasis policy |
| VXLAN | overlay Layer 2 di atas Layer 3 |
| DCI | data center interconnect |
| ZTA | zero trust architecture |
| SASE/SSE | security/network edge berbasis cloud |
| IaC | infrastructure didefinisikan sebagai code |

Use case modern:

| Use Case | Kebutuhan Network |
|---|---|
| IoT | segmentasi, onboarding mudah, monitoring device |
| IIoT/OT | availability tinggi, isolasi ketat, latency predictable |
| Remote work | VPN/ZTNA, MFA, endpoint posture |
| Edge computing | latency rendah dekat user/device |
| Microservices | east-west traffic besar, service discovery |
| Containers/Kubernetes | overlay network, ingress, service IP |
| Automation | API, templates, config validation |

IPv6 transition:

| Metode | Arti |
|---|---|
| Dual stack | host menjalankan IPv4 dan IPv6 |
| Tunneling | IPv6 dibungkus lewat IPv4 atau sebaliknya |
| NAT64 | IPv6 client mengakses IPv4 service melalui translation |

IPv6 address basics:

```text
2001:db8:abcd:10::25/64
```

Field:

| Bagian | Arti |
|---|---|
| `2001:db8` | documentation prefix contoh |
| `abcd:10` | network/subnet portion contoh |
| `::25` | host/interface identifier |
| `/64` | prefix umum untuk LAN IPv6 |

Jenis IPv6:

| Jenis | Prefix | Arti |
|---|---|---|
| Link-local | `fe80::/10` | hanya local link, wajib di interface |
| Global unicast | `2000::/3` | routable global |
| Unique local | `fc00::/7` | private-like IPv6 |
| Multicast | `ff00::/8` | group communication |
| Loopback | `::1/128` | localhost |

Catatan:

- IPv6 tidak memakai broadcast.
- Neighbor Discovery menggantikan banyak fungsi ARP.
- SLAAC memakai Router Advertisement.
- DHCPv6 bisa memberi address atau hanya option tambahan tergantung desain.

### 1.9 IP Addressing Hands-on Deep Dive

IP addressing harus bisa dihitung manual, dibaca dari output command, dan diverifikasi di host/router. Bagian ini fokus ke cara kerja bit `1` dan `0`, netmask, prefix, range, dan perbedaan IPv4/IPv6 secara praktis.

IP version:

| Version | Panjang | Format | Contoh | Catatan |
|---|---:|---|---|---|
| IPv4 | 32 bit | decimal dotted quad | `192.168.10.25/24` | punya broadcast |
| IPv6 | 128 bit | hexadecimal hextet | `2001:db8:abcd:10::25/64` | tidak punya broadcast |

#### IPv4 Binary dan Decimal

IPv4 terdiri dari 32 bit yang dibagi menjadi 4 octet. Satu octet berisi 8 bit.

```text
192.168.10.25

192        168        10         25
11000000 . 10101000 . 00001010 . 00011001
```

Nilai bit dalam satu octet:

| Bit Position | Nilai |
|---:|---:|
| 1 | 128 |
| 2 | 64 |
| 3 | 32 |
| 4 | 16 |
| 5 | 8 |
| 6 | 4 |
| 7 | 2 |
| 8 | 1 |

Cara membaca decimal dari binary:

```text
11000000 = 128 + 64 = 192
10101000 = 128 + 32 + 8 = 168
00001010 = 8 + 2 = 10
00011001 = 16 + 8 + 1 = 25
```

Decimal ke binary octet:

| Decimal | Binary | Perhitungan |
|---:|---|---|
| 0 | `00000000` | tidak ada bit aktif |
| 1 | `00000001` | 1 |
| 2 | `00000010` | 2 |
| 10 | `00001010` | 8 + 2 |
| 25 | `00011001` | 16 + 8 + 1 |
| 128 | `10000000` | 128 |
| 192 | `11000000` | 128 + 64 |
| 224 | `11100000` | 128 + 64 + 32 |
| 240 | `11110000` | 128 + 64 + 32 + 16 |
| 248 | `11111000` | 128 + 64 + 32 + 16 + 8 |
| 252 | `11111100` | 128 + 64 + 32 + 16 + 8 + 4 |
| 254 | `11111110` | semua kecuali bit terakhir |
| 255 | `11111111` | semua bit aktif |

#### IPv4 Netmask, Prefix, dan Host Bit

Netmask memisahkan bagian network dan host.

Aturan:

- Bit `1` pada netmask berarti bagian network.
- Bit `0` pada netmask berarti bagian host.
- Prefix `/24` berarti 24 bit pertama adalah network bit.
- Host bit semua `0` adalah network address.
- Host bit semua `1` adalah broadcast address.

Contoh `/24`:

```text
IP:      192.168.10.25
Binary:  11000000.10101000.00001010.00011001

Mask:    255.255.255.0
Binary:  11111111.11111111.11111111.00000000

Network: 192.168.10.0
Host:    0.0.0.25
```

Network address dihitung dengan operasi AND:

```text
IP bit:    11000000.10101000.00001010.00011001
Mask bit:  11111111.11111111.11111111.00000000
AND:       11000000.10101000.00001010.00000000

Network:  192.168.10.0
```

Operasi AND:

| A | B | A AND B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Prefix ke mask:

| Prefix | Binary Octet yang Berubah | Decimal Mask |
|---:|---|---|
| `/8` | `11111111.00000000.00000000.00000000` | `255.0.0.0` |
| `/16` | `11111111.11111111.00000000.00000000` | `255.255.0.0` |
| `/24` | `11111111.11111111.11111111.00000000` | `255.255.255.0` |
| `/25` | `11111111.11111111.11111111.10000000` | `255.255.255.128` |
| `/26` | `11111111.11111111.11111111.11000000` | `255.255.255.192` |
| `/27` | `11111111.11111111.11111111.11100000` | `255.255.255.224` |
| `/28` | `11111111.11111111.11111111.11110000` | `255.255.255.240` |
| `/29` | `11111111.11111111.11111111.11111000` | `255.255.255.248` |
| `/30` | `11111111.11111111.11111111.11111100` | `255.255.255.252` |
| `/31` | `11111111.11111111.11111111.11111110` | `255.255.255.254` |
| `/32` | `11111111.11111111.11111111.11111111` | `255.255.255.255` |

Host count:

```text
host bits = 32 - prefix
total address = 2 ^ host bits
usable host = total address - 2
```

Catatan:

- `-2` karena network address dan broadcast address tidak dipakai host biasa.
- `/31` bisa dipakai untuk point-to-point modern, sehingga dua address bisa usable di link tersebut.
- `/32` adalah satu host route, bukan subnet LAN biasa.

#### IPv4 Range Calculation

Contoh hitung `192.168.1.130/26`.

Step 1: cari mask.

```text
/26 = 255.255.255.192
```

Step 2: cari block size pada octet yang berubah.

```text
256 - 192 = 64
```

Step 3: subnet boundary di octet terakhir.

```text
0, 64, 128, 192
```

Step 4: `130` masuk range `128 - 191`.

```text
Network:   192.168.1.128
Broadcast: 192.168.1.191
Usable:    192.168.1.129 - 192.168.1.190
```

Binary view:

```text
IP:      192.168.1.130
Binary:  11000000.10101000.00000001.10000010

Mask:    255.255.255.192
Binary:  11111111.11111111.11111111.11000000

Network: 11000000.10101000.00000001.10000000 = 192.168.1.128
Broadcast host bits all 1:
         11000000.10101000.00000001.10111111 = 192.168.1.191
```

Subnet split `/24` menjadi `/26`:

| Subnet | Network | Usable Range | Broadcast |
|---:|---|---|---|
| 1 | `192.168.1.0/26` | `192.168.1.1 - 192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65 - 192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129 - 192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193 - 192.168.1.254` | `192.168.1.255` |

Common subnet table:

| Prefix | Mask | Block Size | Total Address | Usable Host |
|---:|---|---:|---:|---:|
| `/16` | `255.255.0.0` | 1 in third octet | 65,536 | 65,534 |
| `/20` | `255.255.240.0` | 16 in third octet | 4,096 | 4,094 |
| `/21` | `255.255.248.0` | 8 in third octet | 2,048 | 2,046 |
| `/22` | `255.255.252.0` | 4 in third octet | 1,024 | 1,022 |
| `/23` | `255.255.254.0` | 2 in third octet | 512 | 510 |
| `/24` | `255.255.255.0` | 1 in fourth octet | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 4 | 2 |
| `/31` | `255.255.255.254` | 2 | 2 | point-to-point |
| `/32` | `255.255.255.255` | 1 | 1 | host route |

#### IPv4 Wildcard Mask

Wildcard mask adalah kebalikan subnet mask. Banyak dipakai di ACL dan routing protocol tertentu.

```text
Subnet mask:   255.255.255.0
Wildcard mask: 0.0.0.255
```

Cara hitung:

```text
255.255.255.255
- 255.255.255.0
= 0.0.0.255
```

Contoh:

| Network | Subnet Mask | Wildcard |
|---|---|---|
| `192.168.1.0/24` | `255.255.255.0` | `0.0.0.255` |
| `192.168.1.128/26` | `255.255.255.192` | `0.0.0.63` |
| `10.10.0.0/16` | `255.255.0.0` | `0.0.255.255` |

Wildcard matching:

- Bit `0` pada wildcard berarti harus match.
- Bit `1` pada wildcard berarti bebas.

#### IPv4 Special Address Behavior

| Address/Range | Arti | Catatan |
|---|---|---|
| `0.0.0.0` | unspecified/default | bisa berarti semua interface atau default route |
| `0.0.0.0/0` | default route | semua destination jika tidak ada route lebih spesifik |
| `127.0.0.0/8` | loopback | traffic kembali ke host sendiri |
| `169.254.0.0/16` | link-local/APIPA | host gagal DHCP bisa memilih address ini |
| `224.0.0.0/4` | multicast | bukan unicast host biasa |
| `255.255.255.255` | limited broadcast | broadcast local segment |
| network address | host bits semua `0` | identitas subnet |
| broadcast address | host bits semua `1` | semua host di subnet IPv4 |

#### IPv6 Binary, Hex, dan Prefix

IPv6 panjangnya 128 bit. Ditulis sebagai 8 hextet, tiap hextet 16 bit. Satu digit hex = 4 bit.

```text
2001:0db8:abcd:0010:0000:0000:0000:0025
```

Hex ke binary:

| Hex | Binary |
|---|---|
| `0` | `0000` |
| `1` | `0001` |
| `2` | `0010` |
| `3` | `0011` |
| `4` | `0100` |
| `5` | `0101` |
| `6` | `0110` |
| `7` | `0111` |
| `8` | `1000` |
| `9` | `1001` |
| `a` | `1010` |
| `b` | `1011` |
| `c` | `1100` |
| `d` | `1101` |
| `e` | `1110` |
| `f` | `1111` |

IPv6 compression rule:

| Rule | Contoh |
|---|---|
| Leading zero dalam hextet boleh dihapus | `0db8` menjadi `db8` |
| Satu rangkaian hextet `0000` boleh diganti `::` | `2001:db8:abcd:10::25` |
| `::` hanya boleh dipakai sekali | agar tidak ambigu |
| Hex biasanya ditulis lowercase | `db8`, bukan `DB8` |

Contoh:

```text
Full:       2001:0db8:abcd:0010:0000:0000:0000:0025
Compressed: 2001:db8:abcd:10::25
```

IPv6 `/64`:

```text
2001:db8:abcd:10::25/64

Network prefix: 2001:db8:abcd:10::/64
Interface ID:   0000:0000:0000:0025
```

Pada LAN IPv6, `/64` adalah ukuran subnet yang sangat umum karena SLAAC bergantung pada 64-bit interface identifier.

IPv6 host range concept:

```text
Prefix: 2001:db8:abcd:10::/64
First:  2001:db8:abcd:10::1
Last:   2001:db8:abcd:10:ffff:ffff:ffff:ffff
```

Catatan:

- IPv6 tidak punya broadcast address.
- Address `::` berarti unspecified, bukan host biasa.
- `::1` adalah loopback.
- Banyak address dalam subnet tidak berarti harus discan seperti IPv4.

IPv6 prefix planning:

| Allocation | Bisa Dibagi Menjadi | Catatan |
|---|---:|---|
| `/48` | 65,536 subnet `/64` | umum untuk site besar/enterprise |
| `/56` | 256 subnet `/64` | umum untuk site kecil/branch |
| `/60` | 16 subnet `/64` | small office/lab |
| `/64` | 1 LAN subnet | ukuran standar LAN |
| `/127` | point-to-point | dua address untuk link router |
| `/128` | host route | satu IPv6 address |

Nibbles:

- Satu hex digit = 4 bit.
- Subnetting IPv6 paling enak di boundary 4 bit, misalnya `/48`, `/52`, `/56`, `/60`, `/64`.
- Prefix yang tidak jatuh di boundary 4 bit tetap valid, tetapi lebih sulit dibaca manual.

Contoh split `/56` ke `/64`:

```text
Prefix besar: 2001:db8:abcd:1200::/56

/64 subnet:
2001:db8:abcd:1200::/64
2001:db8:abcd:1201::/64
2001:db8:abcd:1202::/64
...
2001:db8:abcd:12ff::/64
```

Total subnet:

```text
/56 ke /64 = 64 - 56 = 8 subnet bits
2 ^ 8 = 256 subnet /64
```

IPv6 address types:

| Type | Prefix | Scope |
|---|---|---|
| Unspecified | `::/128` | tidak dipakai sebagai source normal |
| Loopback | `::1/128` | host lokal |
| Link-local | `fe80::/10` | hanya local link |
| Unique local | `fc00::/7` | internal/private-like |
| Global unicast | `2000::/3` | routable global |
| Multicast | `ff00::/8` | group traffic |
| Solicited-node multicast | `ff02::1:ff00:0/104` | neighbor discovery |

Link-local dengan zone index:

```text
fe80::aabb:ccff:fedd:eeff%eth0
```

`%eth0` diperlukan karena address link-local yang sama bisa ada di banyak interface. OS perlu tahu interface mana yang dipakai.

#### Hands-on IP Commands

Linux:

```bash
# Melihat IPv4 dan IPv6 address pada semua interface.
ip addr show

# Melihat address pada interface tertentu.
ip addr show dev eth0

# Menambahkan IPv4 address sementara ke interface.
ip addr add 192.168.50.10/24 dev eth0

# Menghapus IPv4 address dari interface.
ip addr del 192.168.50.10/24 dev eth0

# Menambahkan IPv6 address sementara ke interface.
ip -6 addr add 2001:db8:50::10/64 dev eth0

# Melihat routing table IPv4.
ip route show

# Melihat routing table IPv6.
ip -6 route show

# Melihat route yang dipilih kernel untuk IPv4 destination.
ip route get 8.8.8.8

# Melihat route yang dipilih kernel untuk IPv6 destination.
ip -6 route get 2001:4860:4860::8888

# Melihat neighbor table IPv4/ARP dan IPv6/ND.
ip neigh show

# Menghitung subnet jika ipcalc tersedia.
ipcalc 192.168.1.130/26
```

Windows PowerShell:

```bash
# Melihat konfigurasi IP lengkap.
ipconfig /all

# Melihat IP address dengan PowerShell.
Get-NetIPAddress

# Melihat routing table IPv4 dan IPv6.
Get-NetRoute

# Melihat neighbor table ARP/ND.
Get-NetNeighbor

# Menguji koneksi TCP ke port tertentu.
Test-NetConnection 192.168.1.10 -Port 443
```

Hands-on checklist:

1. Ambil satu IP/prefix dari laptop atau VM.
2. Ubah IP ke binary.
3. Ubah prefix ke subnet mask.
4. Lakukan operasi AND untuk mencari network address.
5. Set host bits semua `1` untuk mencari broadcast IPv4.
6. Tentukan usable range.
7. Cek hasil manual dengan `ipcalc` jika tersedia.
8. Bandingkan route yang dipilih dengan `ip route get`.
9. Cek neighbor resolution dengan `ip neigh show`.
10. Ulangi konsep prefix untuk IPv6 tanpa broadcast.

---

## 2.0 Network Implementation

**Praktik setelah bab ini:** ubah konsep routing/switching menjadi bukti forwarding path.

```bash
# Melihat route yang dipilih kernel untuk destination tertentu.
ip route get 8.8.8.8

# Melihat neighbor table untuk ARP/ND.
ip neigh show

# Membuat VLAN subinterface sementara untuk memahami 802.1Q di Linux.
sudo ip link add link eth0 name eth0.10 type vlan id 10

# Melihat VLAN subinterface yang dibuat.
ip -d link show eth0.10
```

Catat: gateway, next-hop, interface keluar, MAC neighbor, VLAN ID, dan perbedaan antara routing Layer 3 dan tagging Layer 2.

Bagian ini membahas routing, switching, wireless, dan instalasi fisik.

### 2.1 Routing Technologies

Routing menentukan jalur packet antar network.

Static vs dynamic:

| Jenis | Arti | Cocok Untuk |
|---|---|---|
| Static routing | route ditulis manual | network kecil, default route, route khusus |
| Dynamic routing | router bertukar informasi route | network besar/berubah |

Protocol routing:

| Protocol | Type | Catatan |
|---|---|---|
| OSPF | link-state IGP | umum di enterprise |
| EIGRP | advanced distance-vector IGP | Cisco-heavy environment |
| BGP | path-vector EGP | internet dan antar AS |

OSPF konsep:

| Konsep | Arti |
|---|---|
| Area | domain OSPF untuk membatasi flooding link-state |
| Area 0 | backbone area |
| Neighbor adjacency | hubungan antar router OSPF |
| LSA | link-state advertisement |
| DR/BDR | designated router/backup di multiaccess network |
| Cost | metric OSPF, biasanya berbasis bandwidth |

BGP konsep:

| Konsep | Arti |
|---|---|
| AS | autonomous system |
| ASN | nomor autonomous system |
| eBGP | BGP antar AS |
| iBGP | BGP dalam AS yang sama |
| Prefix advertisement | network yang diumumkan |
| AS path | daftar AS yang dilewati |
| Local preference | preferensi outbound path internal |
| MED | sinyal preferensi inbound ke neighbor |

EIGRP konsep:

| Konsep | Arti |
|---|---|
| Neighbor | router EIGRP yang bertetangga |
| Successor | route terbaik |
| Feasible successor | backup route yang layak |
| Feasible distance | metric terbaik ke destination |
| Reported distance | metric yang dilaporkan neighbor |

IGP vs EGP:

| Jenis | Fungsi | Contoh |
|---|---|---|
| IGP | routing di dalam satu organisasi/autonomous system | OSPF, EIGRP |
| EGP | routing antar autonomous system | BGP |

Administrative distance umum:

| Route Source | AD |
|---|---:|
| Connected | 0 |
| Static | 1 |
| EIGRP internal | 90 |
| OSPF | 110 |
| RIP | 120 |
| External BGP | 20 |
| Internal BGP | 200 |

Catatan:

- Administrative distance dipakai untuk memilih sumber route jika prefix sama.
- Metric dipakai di dalam protocol routing yang sama.
- Longest prefix match tetap menang sebelum AD dibandingkan.

Route selection:

| Faktor | Arti |
|---|---|
| Longest prefix match | route paling spesifik menang |
| Administrative distance | trust sumber route |
| Metric | biaya dalam protocol routing |

Contoh route table:

```text
Destination        Next Hop        Interface
0.0.0.0/0          192.168.1.1     eth0
10.10.0.0/16       10.0.0.2        eth1
192.168.1.0/24     connected       eth0
```

Field:

| Field | Arti |
|---|---|
| Destination | network tujuan |
| Next Hop | router berikutnya |
| Interface | interface keluar |

Contoh static route:

```text
ip route 10.20.0.0/16 192.168.1.254
```

Field static route:

| Field | Contoh | Arti |
|---|---|---|
| destination prefix | `10.20.0.0/16` | network tujuan |
| next hop | `192.168.1.254` | router berikutnya |

Default route:

```text
0.0.0.0/0 via 192.168.1.1
```

Artinya semua traffic yang tidak punya route lebih spesifik dikirim ke `192.168.1.1`.

NAT/PAT:

| Konsep | Arti |
|---|---|
| NAT | translate IP address |
| PAT | translate IP + port, banyak client memakai satu public IP |

Contoh PAT:

| Inside Local | Inside Global |
|---|---|
| `192.168.1.10:51544` | `203.0.113.5:40001` |
| `192.168.1.11:51544` | `203.0.113.5:40002` |

Artinya banyak host private bisa berbagi satu public IP dengan port berbeda.

FHRP/VIP:

| Konsep | Arti |
|---|---|
| FHRP | gateway redundancy |
| VIP | virtual IP yang dipakai client sebagai gateway/service |

FHRP contoh:

```text
Client default gateway: 10.10.10.1
Router A real IP:       10.10.10.2
Router B real IP:       10.10.10.3
Virtual gateway IP:     10.10.10.1
```

Jika Router A gagal, Router B mengambil alih virtual gateway IP sehingga client tidak perlu mengganti default gateway.

### 2.2 Switching Technologies

Switching bekerja di Layer 2 menggunakan MAC address dan frame.

VLAN:

| Konsep | Arti |
|---|---|
| VLAN | logical broadcast domain |
| Access port | port untuk satu VLAN, biasanya end device |
| Trunk port | membawa banyak VLAN |
| Native VLAN | VLAN tanpa tag pada trunk 802.1Q |
| Voice VLAN | VLAN khusus VoIP phone |
| SVI | interface VLAN untuk routing/management |

VLAN atau Virtual LAN memecah satu switch fisik menjadi beberapa broadcast domain logis. Tanpa VLAN, semua host pada switch yang sama cenderung berada dalam satu broadcast domain. Dengan VLAN, user, server, voice, guest, dan management network bisa dipisah walaupun melewati perangkat fisik yang sama.

Yang penting: VLAN bukan firewall. VLAN memisahkan broadcast domain Layer 2, tetapi jika ada routing antar VLAN tanpa policy, host di VLAN berbeda tetap bisa saling akses lewat gateway. Security boundary yang sebenarnya biasanya dibuat dengan ACL, firewall, private VLAN, NAC, atau policy routing.

Broadcast domain:

| Item | Dampak |
|---|---|
| Broadcast ARP | hanya tersebar dalam VLAN yang sama |
| DHCP Discover | hanya tersebar dalam VLAN yang sama kecuali ada DHCP relay |
| MAC table | switch belajar MAC per VLAN |
| STP | bisa berjalan per VLAN tergantung implementasi |
| Failure domain | loop/broadcast storm bisa terbatas pada VLAN tertentu |

Contoh sederhana:

```text
Switch fisik yang sama

Port 1-10  -> VLAN 10 Users
Port 11-15 -> VLAN 20 Voice
Port 16-20 -> VLAN 30 Servers
Port 21-24 -> VLAN 99 Management
```

Jika host di VLAN 10 ingin bicara dengan server di VLAN 30, traffic harus keluar dari Layer 2 domain VLAN 10 menuju gateway Layer 3, lalu dirutekan ke VLAN 30.

Inter-VLAN routing:

| Metode | Arti |
|---|---|
| Router-on-a-stick | router satu interface trunk untuk banyak VLAN |
| Layer 3 switch SVI | switch melakukan routing antar VLAN lewat SVI |
| Firewall as gateway | firewall menjadi default gateway antar zone/VLAN |

Contoh SVI:

```text
interface vlan 10
  ip address 10.10.10.1/24

interface vlan 20
  ip address 10.10.20.1/24
```

Arti:

| Interface | Fungsi |
|---|---|
| `vlan 10` | gateway untuk subnet user VLAN 10 |
| `vlan 20` | gateway untuk subnet voice VLAN 20 |

Contoh VLAN design:

| VLAN | Nama | Subnet | Tujuan |
|---:|---|---|---|
| 10 | Users | `10.10.10.0/24` | laptop/desktop user |
| 20 | Voice | `10.10.20.0/24` | IP phone |
| 30 | Servers | `10.10.30.0/24` | server internal |
| 99 | Management | `10.10.99.0/24` | switch/AP/firewall management |

Access port vs trunk:

| Port Type | Membawa VLAN | Tagging |
|---|---|---|
| Access | satu VLAN | frame biasanya untagged |
| Trunk | banyak VLAN | frame biasanya tagged 802.1Q |

Access port biasanya terhubung ke endpoint seperti laptop, printer, camera, atau IP phone. Frame yang keluar ke endpoint biasanya tidak punya VLAN tag. Switch menambahkan konteks VLAN secara internal berdasarkan konfigurasi port.

Trunk port biasanya menghubungkan switch ke switch, switch ke router, switch ke firewall, switch ke hypervisor, atau switch ke access point. Trunk membawa banyak VLAN dalam satu link fisik dengan tag 802.1Q.

Frame pada access port:

```text
Endpoint -> Switch access port VLAN 10

Ethernet Frame
  Destination MAC: gateway/user/server
  Source MAC: endpoint
  EtherType: IPv4
  Payload: IP packet

Switch menerima frame itu sebagai bagian dari VLAN 10 karena port dikonfigurasi access VLAN 10.
```

Frame pada trunk:

```text
Switch A -> Switch B trunk

Ethernet Frame
  Destination MAC: ...
  Source MAC: ...
  802.1Q Tag:
    VLAN ID: 10
    PCP: priority
    DEI: drop eligible
  EtherType: IPv4
  Payload: IP packet
```

Perbandingan:

| Situasi | Access | Trunk |
|---|---|---|
| Endpoint biasa | ya | tidak |
| Antar switch | tidak | ya |
| Hypervisor host dengan banyak VM VLAN | kadang | ya |
| AP dengan banyak SSID | tidak | ya |
| Firewall/router subinterface | tidak | ya |

Native VLAN:

- Native VLAN adalah VLAN untagged pada trunk.
- Native VLAN mismatch bisa menyebabkan traffic masuk VLAN salah.
- Best practice: jangan pakai VLAN 1 sebagai native VLAN untuk user traffic.

Native VLAN detail:

```text
Trunk link:
  VLAN 10 tagged
  VLAN 20 tagged
  VLAN 99 tagged
  Native VLAN untagged
```

Jika Switch A menganggap native VLAN adalah 999, tetapi Switch B menganggap native VLAN adalah 1, frame untagged yang melewati trunk bisa masuk broadcast domain yang salah. Ini bisa menyebabkan gejala aneh: DHCP salah, device muncul di subnet salah, atau traffic management bocor ke VLAN default.

Praktik yang lebih aman:

- set native VLAN eksplisit
- gunakan native VLAN yang tidak dipakai user
- hindari VLAN 1 untuk user/management
- batasi allowed VLAN pada trunk
- matikan trunk negotiation pada port endpoint
- dokumentasikan VLAN allowed di uplink

Allowed VLAN:

```text
Trunk ke access switch lantai 2
Allowed VLAN: 10,20,99

Trunk ke server hypervisor
Allowed VLAN: 30,40,50
```

Allowed VLAN yang terlalu lebar membuat blast radius lebih besar. Kalau trunk hanya butuh VLAN 10 dan 20, jangan biarkan semua VLAN lewat.

DTP:

- DTP adalah Dynamic Trunking Protocol pada beberapa switch Cisco.
- Jika tidak dibutuhkan, trunk sebaiknya dikonfigurasi eksplisit.
- Port user sebaiknya dipaksa menjadi access port agar tidak bisa dinegosiasikan menjadi trunk.

802.1Q tag:

```text
Ethernet Frame + VLAN Tag

Destination MAC | Source MAC | 802.1Q Tag | EtherType | Payload | FCS
```

Field tag penting:

| Field | Arti |
|---|---|
| TPID | penanda frame bertag VLAN |
| PCP | priority/QoS |
| DEI | drop eligible indicator |
| VLAN ID | nomor VLAN |

Detail field:

| Field | Ukuran | Catatan |
|---|---:|---|
| TPID | 16 bit | biasanya `0x8100`, menandakan frame 802.1Q |
| PCP | 3 bit | priority code point untuk QoS Layer 2 |
| DEI | 1 bit | drop eligible indicator |
| VLAN ID | 12 bit | VLAN ID, secara teori 0-4095 |

VLAN ID umum:

| VLAN ID | Catatan |
|---:|---|
| 0 | priority tag, bukan VLAN biasa |
| 1 | default VLAN pada banyak switch, sebaiknya tidak dipakai untuk user |
| 2-1001 | normal range pada banyak platform |
| 1002-1005 | historically reserved untuk legacy Token Ring/FDDI pada Cisco |
| 1006-4094 | extended range |
| 4095 | reserved |

MAC table per VLAN:

```text
VLAN    MAC Address        Type      Port
10      aaaa.bbbb.cccc     dynamic   Gi1/0/10
20      aaaa.bbbb.cccc     dynamic   Gi1/0/20
```

MAC address yang sama bisa muncul di VLAN berbeda karena lookup Layer 2 memperhitungkan VLAN. Switch tidak hanya berpikir "MAC ini ada di port mana", tetapi "MAC ini ada di port mana untuk VLAN mana".

VLAN dan DHCP:

```text
Client VLAN 10
-> DHCP Discover broadcast
-> Switch tetap dalam VLAN 10
-> SVI/firewall VLAN 10 menerima broadcast
-> DHCP relay mengirim unicast ke DHCP server
-> DHCP server memberi IP scope VLAN 10
```

Jika client mendapat IP dari subnet yang salah, kandidat awal:

| Gejala | Kemungkinan |
|---|---|
| IP subnet salah | port masuk VLAN salah |
| AP SSID memberi subnet salah | SSID-to-VLAN mapping salah |
| VM dapat IP VLAN lain | port group/trunk hypervisor salah |
| DHCP tidak menjawab | DHCP relay tidak ada atau VLAN tidak sampai gateway |
| Gateway tidak bisa ping | access VLAN atau trunk allowed VLAN salah |

VLAN pada virtualization:

| Mode | Arti |
|---|---|
| VLAN di physical switch access port | host/hypervisor hanya melihat untagged VLAN tertentu |
| VLAN trunk ke hypervisor | hypervisor/virtual switch membagi VLAN ke VM/port group |
| VLAN tag di guest VM | guest OS sendiri mengirim frame tagged, butuh desain khusus |

Contoh hypervisor:

```text
Physical switch trunk -> Hypervisor
Allowed VLAN: 30,40

Port group: Web-Servers VLAN 30
Port group: DB-Servers VLAN 40
```

VLAN pada wireless:

```text
SSID Corp    -> VLAN 10
SSID Voice   -> VLAN 20
SSID Guest   -> VLAN 50
SSID Admin   -> VLAN 99
```

AP biasanya terhubung ke switch lewat trunk karena satu AP bisa membawa beberapa SSID/VLAN.

VLAN security:

| Risiko | Penjelasan | Mitigasi |
|---|---|---|
| VLAN hopping switch spoofing | endpoint mencoba menjadi trunk | disable DTP, paksa access mode |
| Double tagging | frame membawa dua tag dan menyalahgunakan native VLAN | native VLAN tidak dipakai user, jangan pakai VLAN 1 |
| Native VLAN mismatch | untagged frame masuk VLAN salah | native VLAN konsisten dan eksplisit |
| Trunk allowed terlalu luas | VLAN tidak perlu ikut tersebar | allowlist VLAN pada trunk |
| Management VLAN exposed | user bisa akses switch/AP/firewall management | ACL, dedicated admin subnet, MFA/AAA |
| Rogue DHCP dalam VLAN | client mendapat gateway/DNS palsu | DHCP snooping |
| ARP spoofing | traffic dialihkan dalam VLAN | Dynamic ARP Inspection |

Troubleshooting VLAN:

| Pertanyaan | Bukti yang Dicari |
|---|---|
| Port endpoint VLAN berapa | switchport mode/access VLAN |
| Trunk membawa VLAN yang benar | allowed VLAN list |
| Native VLAN sama di dua sisi | trunk detail |
| MAC endpoint belajar di VLAN benar | MAC address table |
| Gateway VLAN hidup | SVI/interface status |
| DHCP relay ada | helper address/relay config |
| STP memblokir port | STP state |
| AP/VM mapping benar | SSID/port group VLAN ID |

Command vendor-neutral/conceptual:

```text
# Melihat VLAN yang ada dan port membership.
show vlan

# Melihat konfigurasi access/trunk pada interface.
show interface switchport

# Melihat trunk, native VLAN, dan allowed VLAN.
show interface trunk

# Melihat MAC address dipelajari pada VLAN tertentu.
show mac address-table vlan 10

# Melihat ARP gateway atau host.
show arp

# Melihat status SVI.
show ip interface brief

# Melihat STP state per VLAN.
show spanning-tree vlan 10
```

Command Linux untuk melihat VLAN tag:

```bash
# Melihat VLAN subinterface di Linux.
ip -d link show

# Membuat VLAN subinterface lab VLAN 10 di atas eth0.
sudo ip link add link eth0 name eth0.10 type vlan id 10

# Memberi IP pada VLAN subinterface.
sudo ip addr add 10.10.10.10/24 dev eth0.10

# Mengaktifkan VLAN subinterface.
sudo ip link set eth0.10 up

# Capture frame VLAN 10 jika tag terlihat di host.
sudo tcpdump -i eth0 -e vlan 10
```

Catatan capture: VLAN tag kadang tidak terlihat di host karena NIC VLAN offload. Jika capture tidak menampilkan tag, cek offload atau ambil capture dari switch SPAN/TAP.

Link aggregation:

| Konsep | Arti |
|---|---|
| LACP | protocol untuk bundle beberapa link |
| EtherChannel/Port-channel | logical link gabungan |

LACP hal penting:

- Semua member link harus speed/duplex sama.
- VLAN trunk/access config harus konsisten.
- Jika salah satu sisi tidak match, port-channel bisa suspended atau hanya sebagian link aktif.

Discovery protocol:

| Protocol | Vendor/Scope | Fungsi |
|---|---|---|
| LLDP | open standard | menemukan neighbor Layer 2 |
| CDP | Cisco | menemukan neighbor Cisco |

Informasi discovery:

| Field | Arti |
|---|---|
| Neighbor device | perangkat di ujung link |
| Local interface | port lokal |
| Remote interface | port neighbor |
| Platform/capability | tipe perangkat |
| Management address | IP management neighbor |

STP:

| Konsep | Arti |
|---|---|
| Root bridge | switch pusat logical STP |
| Root port | port terbaik menuju root bridge |
| Designated port | port forwarding pada segment |
| Blocking/discarding | port tidak forwarding untuk mencegah loop |

STP port state:

| State | Arti |
|---|---|
| Blocking/Discarding | tidak forwarding traffic user |
| Listening | mendengar BPDU, belum learning MAC |
| Learning | belajar MAC, belum forwarding traffic user |
| Forwarding | forwarding traffic user |

STP variant:

| Variant | Arti |
|---|---|
| STP | 802.1D, convergence lebih lambat |
| RSTP | 802.1w, convergence lebih cepat |
| MSTP | 802.1s, mapping banyak VLAN ke instance |
| PVST/RPVST | per-VLAN spanning tree, umum di Cisco |

Root bridge selection:

- Switch dengan bridge ID terendah menjadi root bridge.
- Bridge ID terdiri dari priority dan MAC address.
- Admin biasanya mengatur priority agar root bridge berada di switch distribution/core yang benar.

MAC address table:

```text
VLAN    MAC Address        Type      Port
10      aaaa.bbbb.cccc     dynamic   Gi1/0/10
20      dddd.eeee.ffff     dynamic   Gi1/0/20
```

Field:

| Field | Arti |
|---|---|
| VLAN | VLAN tempat MAC dipelajari |
| MAC Address | alamat Layer 2 host |
| Type | dynamic/static |
| Port | port tempat MAC terlihat |

MTU:

- MTU adalah ukuran frame/payload maksimum.
- Jumbo frame biasanya sekitar 9000 bytes.
- MTU mismatch bisa membuat traffic besar gagal tetapi ping kecil berhasil.

### 2.3 Wireless Devices and Technologies

Wireless memakai radio, sehingga interference, channel, power, dan coverage sangat penting.

Frequency:

| Band | Kelebihan | Kekurangan |
|---|---|---|
| 2.4 GHz | jangkauan lebih jauh | channel sedikit, banyak interference |
| 5 GHz | channel lebih banyak, lebih cepat | jangkauan lebih pendek |
| 6 GHz | spektrum baru, lebih bersih | butuh device modern |

802.11 standards:

| Standard | Band Umum | Catatan |
|---|---|---|
| 802.11a | 5 GHz | lama |
| 802.11b/g | 2.4 GHz | lama, rawan interference |
| 802.11n | 2.4/5 GHz | Wi-Fi 4 |
| 802.11ac | 5 GHz | Wi-Fi 5 |
| 802.11ax | 2.4/5/6 GHz | Wi-Fi 6/6E |
| 802.11be | 2.4/5/6 GHz | Wi-Fi 7 |

Channel:

| Konsep | Arti |
|---|---|
| Channel width | lebar channel, misalnya 20/40/80 MHz |
| Non-overlapping channel | channel yang tidak saling tumpang tindih |
| Regulatory domain | aturan negara untuk channel/power |
| 802.11h | DFS/TPC untuk menghindari radar |

2.4 GHz non-overlapping:

| Channel | Catatan |
|---:|---|
| 1 | umum dipakai |
| 6 | umum dipakai |
| 11 | umum dipakai |

Channel width:

| Width | Catatan |
|---|---|
| 20 MHz | stabil, cocok area padat |
| 40 MHz | throughput lebih tinggi, lebih mudah overlap |
| 80 MHz | throughput tinggi, butuh spectrum bersih |
| 160 MHz | sangat lebar, rawan overlap/DFS |

Band steering:

- Mendorong client yang mendukung 5/6 GHz agar tidak menumpuk di 2.4 GHz.
- Tidak semua client mengikuti arahan AP dengan baik.

SSID/BSSID/ESSID:

| Istilah | Arti |
|---|---|
| SSID | nama network Wi-Fi |
| BSSID | MAC radio AP untuk SSID tertentu |
| ESSID | SSID yang sama diperluas oleh banyak AP |

Wireless mode:

| Mode | Arti |
|---|---|
| Infrastructure | client terhubung ke AP |
| Ad hoc | client langsung ke client |
| Mesh | AP saling terhubung wireless |
| Point-to-point | link wireless dua titik |

Security:

| Mode | Arti |
|---|---|
| WPA2-Personal | PSK/password bersama |
| WPA3-Personal | PSK modern dengan SAE |
| Enterprise | autentikasi per user via 802.1X/RADIUS |

WPA-Enterprise flow:

```text
Client -> AP -> RADIUS server
```

Alur:

1. Client memilih SSID enterprise.
2. AP bertindak sebagai authenticator.
3. Client melakukan EAP authentication.
4. RADIUS memvalidasi credential/certificate.
5. Client mendapat akses dan bisa ditempatkan ke VLAN/policy tertentu.

EAP method umum:

| Method | Arti |
|---|---|
| PEAP | password dilindungi tunnel TLS |
| EAP-TLS | memakai client certificate |
| EAP-TTLS | tunnel TLS dengan credential internal |

Wireless design metrics:

| Metric | Arti |
|---|---|
| RSSI | kekuatan sinyal diterima |
| SNR | signal-to-noise ratio |
| Noise floor | tingkat noise radio |
| Channel utilization | seberapa sibuk channel |
| Roaming | perpindahan client antar AP |

Roaming assist:

| Standard | Fungsi |
|---|---|
| 802.11k | memberi informasi neighbor AP |
| 802.11v | membantu steering client |
| 802.11r | fast transition untuk roaming lebih cepat |

Antennas:

| Tipe | Pola |
|---|---|
| Omnidirectional | menyebar ke banyak arah |
| Directional | fokus ke arah tertentu |

Captive portal:

- Captive portal memaksa user membuka halaman login/accept policy sebelum internet.
- Umum untuk guest Wi-Fi.
- Tidak sama dengan encryption; guest tetap perlu dipisah dari internal network.

Guest network:

- Biasanya dipisah VLAN/subnet dari internal.
- Sering memakai captive portal.
- Harus dibatasi agar tidak bisa akses management/internal system.

AP:

| Tipe | Arti |
|---|---|
| Autonomous AP | dikelola sendiri |
| Lightweight AP | dikontrol controller/cloud |

### 2.4 Physical Installations

Instalasi fisik menentukan reliability network.

Lokasi:

| Lokasi | Arti |
|---|---|
| MDF | main distribution frame, pusat distribusi utama |
| IDF | intermediate distribution frame, distribusi per area/lantai |

Rack dan cabling:

| Item | Fungsi |
|---|---|
| Rack unit/U | ukuran tinggi perangkat |
| Patch panel | terminasi kabel horizontal |
| Fiber distribution panel | terminasi fiber |
| Cable management | menjaga airflow dan tracing kabel |
| Lockable rack | keamanan fisik |

Cable rating:

| Rating | Penggunaan |
|---|---|
| PVC | area umum, bukan plenum |
| Plenum | ruang airflow/plenum, bahan lebih aman saat terbakar |
| Riser | jalur vertikal antar lantai |
| Outdoor-rated | tahan kondisi luar ruangan |

Termination tools:

| Tool | Fungsi |
|---|---|
| Punchdown tool | terminasi kabel ke patch panel/keystone |
| Crimper | memasang connector RJ45 |
| Cable stripper | membuka jacket kabel |
| Loopback plug | menguji port/transceiver tertentu |
| OTDR | mengukur karakteristik dan fault fiber |

Patch panel flow:

```text
Wall jack -> horizontal cable -> patch panel -> patch cable -> switch port
```

Field cable map:

| Field | Contoh | Arti |
|---|---|---|
| Room/Jack | `2F-A12` | lokasi end user |
| Patch panel | `PP1-24` | port patch panel |
| Switch | `SW-ACC-01 Gi1/0/24` | port switch |
| VLAN | `10` | VLAN port |
| Label | `USER-2F-A12` | label fisik/logis |

Labeling best practice:

| Label | Contoh | Arti |
|---|---|---|
| Site | `JKT-HQ` | lokasi |
| Rack | `R02` | rack |
| Patch panel | `PP1` | panel |
| Port | `24` | port panel/switch |
| Destination | `2F-A12` | tujuan wall jack/device |

Contoh label:

```text
JKT-HQ-R02-PP1-24_to_2F-A12
```

Power:

| Item | Fungsi |
|---|---|
| UPS | backup power sementara |
| PDU | distribusi listrik ke rack |
| Power load | total beban listrik |
| Voltage | tegangan input |

PoE:

| Standard | Power Class Umum | Catatan |
|---|---|---|
| 802.3af | PoE | IP phone/AP kecil |
| 802.3at | PoE+ | AP lebih besar, camera |
| 802.3bt | PoE++ | device power tinggi |

Power budget:

- Switch punya total power budget.
- Tiap PoE device mengambil sebagian budget.
- Jika budget habis, device baru bisa tidak menyala walaupun port network aktif.

Environment:

- Temperature terlalu tinggi memperpendek umur perangkat.
- Humidity terlalu tinggi/rendah bisa merusak perangkat.
- Fire suppression harus cocok untuk ruang perangkat.
- Port-side intake/exhaust harus sesuai airflow data center.

---

## 3.0 Network Operations

**Praktik setelah bab ini:** kumpulkan bukti operasional sebelum membuat kesimpulan.

```bash
# Menguji reachability dan latency dasar.
ping -c 4 8.8.8.8

# Melacak jalur packet ke tujuan.
traceroute 8.8.8.8

# Capture traffic DNS untuk melihat query dan response.
sudo tcpdump -i eth0 -nn port 53
```

Catat: waktu tes, scope dampak, hop yang berubah, packet loss, latency, DNS resolver, dan perubahan terakhir yang mungkin relevan.

Bagian ini membahas dokumentasi, monitoring, DR, network services, dan akses management.

### 3.1 Organizational Processes and Procedures

Dokumentasi membuat network bisa dioperasikan oleh tim, bukan hanya oleh orang yang membangunnya.

Jenis dokumentasi:

| Dokumen | Fungsi |
|---|---|
| Physical diagram | lokasi device dan kabel |
| Logical diagram | subnet, VLAN, routing, firewall zone |
| Rack diagram | posisi perangkat dalam rack |
| Cable map | port ke patch panel/device |
| IPAM | inventaris IP, subnet, DHCP scope |
| Asset inventory | hardware, software, license, warranty |
| Wireless survey/heat map | coverage dan signal Wi-Fi |

Contoh IPAM record:

| Field | Contoh | Arti |
|---|---|---|
| Subnet | `10.10.20.0/24` | network yang dikelola |
| VLAN | `20` | VLAN terkait |
| Gateway | `10.10.20.1` | default gateway subnet |
| DHCP Scope | `10.10.20.50-10.10.20.200` | range dynamic |
| Reserved | `10.10.20.10-10.10.20.49` | server/printer/static |
| Site | `Jakarta-HQ` | lokasi |
| Owner | `Network Team` | penanggung jawab |

Asset inventory:

| Field | Contoh | Arti |
|---|---|---|
| Hostname | `SW-ACC-01` | nama perangkat |
| Serial | `FOC1234ABCD` | serial number |
| Model | `C9300-48P` | model hardware |
| OS/Firmware | `17.x` | versi software |
| Location | `IDF-2F` | lokasi fisik |
| Warranty | `2028-12-31` | masa support |
| Role | `Access Switch` | fungsi perangkat |

SLA dan lifecycle:

| Item | Arti | Catatan |
|---|---|---|
| SLA | service-level agreement | target availability, response time, support |
| EOL | end-of-life | vendor tidak lagi menjual/aktif mengembangkan produk |
| EOS | end-of-support | support/security update berhenti |
| Patch | update untuk bug/security/performance |
| Firmware | software low-level perangkat network |
| OS image | sistem operasi perangkat/server |
| Decommissioning | proses retire perangkat/service dengan aman |

Decommission checklist:

1. Validasi perangkat/service tidak lagi dipakai.
2. Backup konfigurasi dan export dokumentasi.
3. Hapus route, DNS, DHCP reservation, monitoring, dan firewall rule terkait.
4. Cabut akses credential/API/token.
5. Sanitasi storage/config jika perangkat keluar dari organisasi.
6. Update asset inventory dan diagram.

Change management:

| Tahap | Arti |
|---|---|
| Request | perubahan diajukan |
| Review | risiko dan impact dinilai |
| Approval | perubahan disetujui |
| Implementation | perubahan dilakukan |
| Validation | hasil dicek |
| Rollback | kembali jika gagal |
| Documentation | perubahan dicatat |

Field change request yang bagus:

| Field | Arti |
|---|---|
| Change ID | nomor unik perubahan |
| Scope | perangkat, site, VLAN, subnet, atau service yang terkena |
| Reason | kenapa perubahan dibutuhkan |
| Risk | risiko teknis dan bisnis |
| Impact | siapa yang terdampak dan berapa lama |
| Implementation steps | langkah perubahan yang akan dilakukan |
| Validation steps | cara membuktikan perubahan berhasil |
| Rollback plan | cara kembali ke kondisi sebelumnya |
| Maintenance window | waktu perubahan yang disetujui |
| Approver | pihak yang menyetujui |

Contoh rollback plan:

```text
Change: ubah default route firewall branch
Rollback trigger: user branch tidak bisa akses aplikasi utama setelah 10 menit
Rollback action: restore route lama dan clear connection state jika perlu
Validation: ping gateway upstream, test HTTPS aplikasi utama, cek tunnel VPN
```

Baseline/golden config:

- Production config adalah konfigurasi aktif.
- Backup config adalah salinan konfigurasi.
- Baseline/golden config adalah konfigurasi standar yang disetujui.
- Configuration drift adalah perbedaan dari baseline.

Backup konfigurasi:

| Jenis | Arti | Catatan |
|---|---|---|
| Manual backup | admin export/copy config sendiri | cocok sebelum change |
| Scheduled backup | backup otomatis berkala | cocok untuk audit dan recovery |
| Versioned backup | backup disimpan dengan riwayat versi | memudahkan diff/rollback |
| Offsite backup | backup disimpan di lokasi berbeda | penting untuk disaster recovery |

Field backup config:

| Field | Contoh | Arti |
|---|---|---|
| Device | `FW-HQ-01` | perangkat asal config |
| Timestamp | `2026-05-23 22:00` | waktu backup |
| Config version | `pre-change-CHG-1021` | label versi |
| Hash | `sha256:...` | bukti integritas file |
| Owner | `Network Team` | penanggung jawab |
| Restore tested | `yes/no` | apakah pernah diuji restore |

Runbook:

- Runbook adalah langkah operasional yang bisa diikuti orang lain.
- Runbook harus cukup jelas untuk dipakai saat incident, bukan hanya saat kondisi tenang.
- Runbook yang baik punya prerequisite, langkah, expected result, rollback, dan kontak eskalasi.

Contoh runbook singkat:

```text
Runbook: DHCP scope exhaustion

1. Cek jumlah lease aktif.
2. Cek apakah banyak lease dari MAC/vendor tidak dikenal.
3. Cek apakah ada rogue device atau loop.
4. Tambah scope sementara jika disetujui.
5. Dokumentasikan subnet, waktu, dan penyebab.
```

### 3.2 Network Monitoring

Monitoring menjawab apakah network sehat, lambat, penuh, atau berubah.

Metode:

| Metode | Fungsi |
|---|---|
| SNMP | membaca metric perangkat |
| SNMP trap | device mengirim event ke collector |
| Flow data | ringkasan percakapan traffic |
| Packet capture | melihat packet detail |
| Baseline metric | kondisi normal pembanding |
| Log aggregation | mengumpulkan log dari banyak device |
| SIEM | korelasi event security |
| API integration | monitoring lewat API |
| Port mirroring | copy traffic ke analyzer |

SNMP:

| Versi | Catatan |
|---|---|
| v2c | community string, tidak aman modern |
| v3 | authentication dan encryption |

SNMP object:

| Konsep | Arti |
|---|---|
| Manager | server monitoring yang query device |
| Agent | service SNMP di device |
| MIB | database object yang bisa dibaca |
| OID | identifier object tertentu |
| Community string | shared string SNMP v1/v2c |
| Trap | event dikirim device ke manager |

Contoh OID umum:

| OID/Name | Fungsi |
|---|---|
| `sysName` | nama device |
| `ifInOctets` | byte masuk interface |
| `ifOutOctets` | byte keluar interface |
| `ifOperStatus` | status operasional interface |

Syslog severity:

| Severity | Nama | Arti |
|---:|---|---|
| 0 | Emergency | system unusable |
| 1 | Alert | action immediately |
| 2 | Critical | critical condition |
| 3 | Error | error condition |
| 4 | Warning | warning condition |
| 5 | Notice | normal but significant |
| 6 | Informational | informational |
| 7 | Debug | debug-level message |

Flow data vs packet capture:

| Teknik | Melihat | Cocok Untuk |
|---|---|---|
| Flow | siapa bicara dengan siapa, volume, port | traffic trend |
| Packet capture | isi packet/header detail | troubleshooting detail |

Flow record field:

| Field | Arti |
|---|---|
| Source IP | asal traffic |
| Destination IP | tujuan traffic |
| Source port | port asal |
| Destination port | port tujuan |
| Protocol | TCP/UDP/ICMP |
| Bytes/packets | volume traffic |
| Start/end time | waktu flow |

Monitoring solution:

| Solution | Tujuan |
|---|---|
| Network discovery | menemukan device |
| Traffic analysis | melihat siapa memakai bandwidth |
| Performance monitoring | latency, utilization, error |
| Availability monitoring | up/down status |
| Configuration monitoring | perubahan konfigurasi |

### 3.3 Disaster Recovery

DR adalah persiapan agar network/service bisa pulih setelah gangguan besar.

Metric:

| Metric | Arti |
|---|---|
| RPO | seberapa banyak data boleh hilang |
| RTO | seberapa lama downtime boleh terjadi |
| MTTR | rata-rata waktu perbaikan |
| MTBF | rata-rata waktu antar kegagalan |

DR site:

| Site | Kesiapan | Biaya |
|---|---|---|
| Cold site | ruangan/fasilitas saja | rendah |
| Warm site | sebagian sistem siap | sedang |
| Hot site | siap hampir real-time | tinggi |

HA:

| Model | Arti |
|---|---|
| Active-active | semua node aktif melayani traffic |
| Active-passive | node standby aktif saat failover |

Testing:

- Tabletop exercise membahas skenario tanpa benar-benar failover.
- Validation test membuktikan proses recovery benar-benar jalan.
- DR plan yang tidak pernah diuji belum bisa dipercaya.

Backup strategy:

| Konsep | Arti |
|---|---|
| Full backup | semua data/config dibackup |
| Incremental backup | hanya perubahan sejak backup terakhir |
| Differential backup | perubahan sejak full backup terakhir |
| Snapshot | titik kondisi cepat untuk restore |
| Immutable backup | backup tidak bisa diubah/dihapus selama periode tertentu |

Network DR dependency:

| Dependency | Kenapa Penting |
|---|---|
| DNS | service recovery gagal jika nama tidak resolve |
| DHCP | client tidak dapat IP setelah recovery |
| Routing | site cadangan harus punya jalur ke user/server |
| Firewall rules | traffic recovery bisa terblokir walau server sudah hidup |
| VPN/tunnel | branch atau remote user butuh jalur aman |
| Certificates | load balancer/VPN bisa gagal jika certificate expired |
| Monitoring | tim harus tahu recovery benar-benar sehat |

HA failover flow:

```text
1. Primary device gagal health check.
2. Peer standby mendeteksi failure.
3. Virtual IP/MAC berpindah atau route berubah.
4. Traffic baru masuk ke standby yang menjadi active.
5. Admin validasi service dan mencari root cause.
```

Hal yang perlu diuji saat failover:

| Item | Pertanyaan |
|---|---|
| State sync | apakah koneksi aktif tetap jalan atau harus reconnect |
| Route convergence | berapa lama routing pulih |
| ARP/ND update | apakah client belajar MAC gateway baru |
| DNS TTL | apakah nama service cepat pindah ke site cadangan |
| Monitoring alert | apakah alert muncul dan clear dengan benar |

Contoh DR runbook:

```text
Scenario: link ISP utama down

Trigger:
  - monitoring loss > 80% selama 5 menit
  - BGP/session ISP utama down

Action:
  - verifikasi dari router edge
  - cek status backup ISP
  - pastikan default route/BGP failover aktif
  - informasikan helpdesk tentang potensi latency

Validation:
  - user bisa akses aplikasi utama
  - DNS publik masih reachable
  - VPN branch tetap up
```

### 3.4 IPv4 and IPv6 Network Services

DHCP memberi IP otomatis.

DHCP concept:

| Konsep | Arti |
|---|---|
| Scope | range IP yang bisa diberikan |
| Lease time | durasi pinjam IP |
| Reservation | IP tetap berdasarkan MAC/client ID |
| Exclusion | IP dalam scope yang tidak diberikan |
| Option | informasi tambahan seperti gateway/DNS |
| Relay/IP helper | meneruskan DHCP antar subnet |

DHCP DORA:

| Step | Packet | Arah | Arti |
|---:|---|---|---|
| 1 | Discover | client -> broadcast | client mencari DHCP server |
| 2 | Offer | server -> client | server menawarkan IP |
| 3 | Request | client -> server/broadcast | client meminta IP yang ditawarkan |
| 4 | Acknowledge | server -> client | server menyetujui lease |

Field DHCP lease:

| Field | Contoh | Arti |
|---|---|---|
| IP address | `10.10.10.50` | address yang diberikan |
| Subnet mask | `255.255.255.0` | subnet mask |
| Router option | `10.10.10.1` | default gateway |
| DNS option | `10.10.1.10` | DNS resolver |
| Lease time | `8 hours` | durasi lease |
| Server ID | `10.10.1.5` | DHCP server |

DHCP relay:

- DHCP broadcast tidak melewati router secara default.
- Relay/IP helper menerima broadcast client dan meneruskannya sebagai unicast ke DHCP server.
- Relay biasanya dikonfigurasi di SVI/router interface subnet client.

DHCP option umum:

| Option | Fungsi |
|---:|---|
| 3 | default gateway/router |
| 6 | DNS server |
| 15 | DNS domain name |
| 42 | NTP server |
| 66 | TFTP/server name, sering untuk phone/PXE |
| 67 | bootfile name, sering untuk PXE |

DHCP troubleshooting clues:

| Gejala | Kemungkinan |
|---|---|
| Client dapat `169.254.x.x` | DHCP tidak reachable atau gagal |
| Client dapat IP tapi tidak internet | gateway option salah atau routing/NAT bermasalah |
| Client dapat DNS salah | option 6 salah atau rogue DHCP |
| Lease cepat habis | scope terlalu kecil atau lease time terlalu panjang |
| Device tertentu selalu salah IP | reservation salah MAC/client ID |

DNS record:

| Record | Fungsi | Contoh |
|---|---|---|
| A | nama ke IPv4 | `app.example.com -> 192.0.2.10` |
| AAAA | nama ke IPv6 | `app.example.com -> 2001:db8::10` |
| CNAME | alias ke nama lain | `www -> app.example.com` |
| MX | mail exchanger | mail server domain |
| TXT | text data | SPF, verification |
| NS | nameserver authoritative | DNS server domain |
| PTR | IP ke nama | reverse lookup |
| SOA | authority metadata | primary zone data |

Contoh zone DNS:

```text
example.com.        NS      ns1.example.com.
example.com.        MX 10   mail.example.com.
www.example.com.    A       203.0.113.10
api.example.com.    CNAME   www.example.com.
example.com.        TXT     "v=spf1 include:_spf.example.com ~all"
```

Field DNS record:

| Field | Contoh | Arti |
|---|---|---|
| Name | `www.example.com.` | nama record |
| Type | `A` | jenis record |
| Value | `203.0.113.10` | nilai record |
| TTL | `300` | berapa lama boleh dicache |
| Priority | `10` | prioritas, umum pada MX/SRV |

Zone:

| Zone | Arti |
|---|---|
| Forward | nama ke IP |
| Reverse | IP ke nama |
| Primary | writable/master zone |
| Secondary | copy dari primary |

DNS behavior:

| Jenis | Arti |
|---|---|
| Authoritative | server punya jawaban resmi |
| Recursive | server mencarikan jawaban untuk client |
| Non-authoritative | jawaban dari cache/recursive resolver |

DNS lookup flow:

```text
Client
-> recursive resolver
-> root server
-> TLD server
-> authoritative nameserver
-> answer kembali ke client lewat resolver
```

DNS caching:

| Item | Arti |
|---|---|
| TTL tinggi | query lebih sedikit, perubahan record lebih lambat terasa |
| TTL rendah | perubahan cepat, query ke DNS lebih banyak |
| Negative cache | jawaban tidak ada record juga bisa dicache |

Split DNS:

- Nama yang sama bisa punya jawaban berbeda untuk internal dan external user.
- Contoh: `app.example.com` dari kantor resolve ke IP private, dari internet resolve ke IP public.
- Salah split DNS bisa membuat user internal keluar ke internet dulu untuk mengakses service internal.

Hosts file:

| OS | Lokasi Umum |
|---|---|
| Linux/macOS | `/etc/hosts` |
| Windows | `C:\Windows\System32\drivers\etc\hosts` |

Contoh:

```text
192.0.2.10 app.example.com
```

Catatan:

- Hosts file biasanya dicek sebelum DNS resolver.
- Entry lama di hosts file bisa membuat troubleshooting DNS membingungkan.
- Jangan jadikan hosts file sebagai solusi permanen untuk production service kecuali ada alasan operasional yang jelas.

Secure DNS:

| Teknologi | Fungsi |
|---|---|
| DNSSEC | validasi integritas jawaban DNS |
| DoH | DNS over HTTPS |
| DoT | DNS over TLS |

Time:

| Protocol | Fungsi |
|---|---|
| NTP | sinkronisasi waktu umum |
| PTP | presisi tinggi |
| NTS | security untuk NTP |

NTP stratum:

| Stratum | Arti |
|---:|---|
| 0 | reference clock, misalnya atomic/GPS |
| 1 | server langsung ke stratum 0 |
| 2+ | server yang sync dari stratum lebih rendah |

Waktu yang tidak sinkron bisa menyebabkan:

- certificate dianggap invalid
- Kerberos/authentication gagal
- log timeline membingungkan
- distributed system error

IPv6:

| Konsep | Arti |
|---|---|
| SLAAC | host membuat address sendiri dari router advertisement |
| Link-local | `fe80::/10`, wajib ada di interface IPv6 |
| Global unicast | address routable global |
| Multicast | pengganti banyak fungsi broadcast IPv4 |

IPv6 Neighbor Discovery:

| Fungsi | IPv4 Setara | Arti |
|---|---|---|
| Neighbor Solicitation | ARP request | mencari MAC address neighbor |
| Neighbor Advertisement | ARP reply | jawaban MAC address neighbor |
| Router Solicitation | tidak langsung sama | host mencari router |
| Router Advertisement | tidak langsung sama | router mengumumkan prefix/default gateway |

DHCPv6:

| Mode | Arti |
|---|---|
| Stateless DHCPv6 | address dari SLAAC, option seperti DNS dari DHCPv6 |
| Stateful DHCPv6 | address dan option diberikan DHCPv6 |
| Prefix delegation | router downstream mendapat prefix IPv6 untuk dibagikan lagi |

### 3.5 Network Access and Management Methods

Access management menentukan cara admin mengelola perangkat.

Metode:

| Metode | Fungsi | Catatan |
|---|---|---|
| SSH | CLI terenkripsi | umum untuk router/switch/server |
| GUI | web management | mudah, perlu HTTPS/MFA |
| API | automation | cocok untuk IaC |
| Console | akses fisik/serial | recovery saat network down |
| Jump box | host perantara admin | membatasi akses management |

In-band vs out-of-band:

| Jenis | Arti |
|---|---|
| In-band | management lewat network produksi |
| Out-of-band | management lewat jalur terpisah |

Management plane, control plane, data plane:

| Plane | Fungsi | Contoh |
|---|---|---|
| Management plane | admin mengelola perangkat | SSH, HTTPS GUI, SNMP, API |
| Control plane | perangkat membangun keputusan forwarding | routing protocol, STP, ARP/ND |
| Data plane | traffic user benar-benar lewat | forwarding packet/frame |

Kenapa dipisah:

- Serangan ke management plane bisa mengambil alih perangkat.
- Gangguan control plane bisa membuat route/STP tidak stabil.
- Data plane bisa penuh traffic walau management masih bisa login, atau sebaliknya.

In-band example:

```text
Admin laptop -> user LAN -> core switch -> management IP firewall
```

Out-of-band example:

```text
Admin VPN/OOB modem -> console server -> serial console router
```

Management access best practice:

| Praktik | Alasan |
|---|---|
| Pakai SSH/HTTPS | Telnet/HTTP tidak terenkripsi |
| Batasi source IP admin | mengurangi permukaan serangan |
| Pakai AAA terpusat | akses bisa diaudit dan dicabut |
| Pakai MFA untuk portal/VPN | mengurangi risiko credential dicuri |
| Logging command/config change | membantu audit dan incident response |
| Backup local admin break-glass | tetap bisa recovery saat AAA down |

VPN:

| Jenis | Arti |
|---|---|
| Site-to-site | menghubungkan network antar site |
| Client-to-site | user remote ke network kantor |
| Clientless | akses lewat browser/portal |
| Split tunnel | hanya traffic tertentu lewat VPN |
| Full tunnel | semua traffic client lewat VPN |

Split tunnel vs full tunnel:

| Mode | Route Client | Dampak |
|---|---|---|
| Split tunnel | hanya prefix kantor lewat VPN | hemat bandwidth, internet langsung |
| Full tunnel | `0.0.0.0/0` lewat VPN | kontrol security lebih besar, beban VPN naik |

Contoh route split tunnel:

```text
10.10.0.0/16 via VPN
10.20.0.0/16 via VPN
0.0.0.0/0 via local internet
```

Contoh route full tunnel:

```text
0.0.0.0/0 via VPN
```

AAA untuk device administration:

| Komponen | Arti |
|---|---|
| Authentication | siapa admin ini |
| Authorization | command atau privilege apa yang boleh dipakai |
| Accounting | command/login dicatat untuk audit |

TACACS+ vs RADIUS:

| Fitur | TACACS+ | RADIUS |
|---|---|---|
| Umum untuk | admin perangkat network | network access/VPN/Wi-Fi |
| Transport | TCP | UDP |
| Authorization command | kuat/detail | lebih terbatas |
| Enkripsi payload | lebih luas | terutama password |

---

## 4.0 Network Security

**Praktik setelah bab ini:** lihat security control sebagai traffic yang diizinkan, ditolak, atau dicatat.

```bash
# Melihat listening socket lokal yang menjadi attack surface.
ss -tulpn

# Melihat ruleset firewall nftables jika tersedia.
sudo nft list ruleset

# Scan port host milik sendiri untuk validasi exposure.
nmap -sV 127.0.0.1
```

Catat: service yang expose port, rule yang mengizinkan traffic, rule yang menolak traffic, dan apakah hasil scan sesuai ekspektasi hardening.

Bagian ini membahas konsep security, attack, dan defense.

### 4.1 Basic Network Security Concepts

Logical security:

| Konsep | Arti |
|---|---|
| Encryption in transit | melindungi data saat dikirim |
| Encryption at rest | melindungi data saat disimpan |
| PKI | sistem certificate dan trust |
| Self-signed certificate | certificate ditandatangani sendiri |

Physical security:

| Kontrol | Fungsi |
|---|---|
| Camera/CCTV | monitoring aktivitas fisik |
| Locks | membatasi akses rack/ruangan |
| Badge access | audit siapa masuk area tertentu |
| Visitor log | mencatat tamu/vendor |

Deception technologies:

| Teknologi | Arti |
|---|---|
| Honeypot | sistem umpan untuk menarik/mendeteksi attacker |
| Honeynet | kumpulan honeypot/network umpan |

IAM:

| Konsep | Arti |
|---|---|
| Authentication | membuktikan identitas |
| Authorization | menentukan hak akses |
| MFA | faktor tambahan selain password |
| SSO | satu login untuk banyak aplikasi |
| RADIUS | AAA untuk network access |
| LDAP | directory service |
| SAML | federation/SSO assertion |
| TACACS+ | AAA untuk device administration |
| RBAC | role-based access control |
| Least privilege | akses minimum yang diperlukan |
| Time-based authentication | akses dipengaruhi waktu atau token berbasis waktu |
| Geofencing | akses dipengaruhi lokasi geografis |

Time-based authentication:

- TOTP menghasilkan one-time code berbasis waktu.
- Policy akses juga bisa dibatasi berdasarkan jam kerja atau maintenance window.
- Waktu client/server harus sinkron agar token berbasis waktu tidak gagal.

Compliance dan data locality:

| Item | Arti |
|---|---|
| Data locality | aturan/lokasi tempat data boleh disimpan/diproses |
| PCI DSS | standar keamanan untuk card/payment data |
| GDPR | regulasi perlindungan data pribadi Uni Eropa |

Catatan:

- Compliance bukan pengganti security engineering.
- Network design bisa dipengaruhi lokasi data, logging, encryption, segmentation, dan akses admin.

Security terminology:

| Istilah | Arti |
|---|---|
| Risk | kemungkinan kerugian |
| Vulnerability | kelemahan |
| Exploit | cara memanfaatkan kelemahan |
| Threat | aktor/kondisi yang bisa menyerang |
| CIA | confidentiality, integrity, availability |

CIA triad:

| Komponen | Arti | Contoh Gangguan |
|---|---|---|
| Confidentiality | data hanya dilihat pihak berwenang | password dikirim cleartext |
| Integrity | data tidak berubah tanpa izin | DNS record dipalsukan |
| Availability | service tersedia saat dibutuhkan | DDoS membuat website down |

Risk relationship:

```text
Threat memanfaatkan vulnerability dengan exploit sehingga menimbulkan risk.
```

Contoh:

| Item | Contoh |
|---|---|
| Asset | VPN gateway |
| Threat | attacker internet |
| Vulnerability | firmware lama punya CVE |
| Exploit | request khusus untuk remote code execution |
| Risk | attacker mengambil akses VPN |
| Mitigation | patch firmware, restrict source IP, MFA, monitoring |

AAA:

| Komponen | Pertanyaan yang Dijawab |
|---|---|
| Authentication | siapa kamu |
| Authorization | kamu boleh melakukan apa |
| Accounting | apa yang kamu lakukan dan kapan |

Authentication factor:

| Factor | Contoh |
|---|---|
| Something you know | password/PIN |
| Something you have | hardware token, authenticator app |
| Something you are | fingerprint/biometric |
| Somewhere you are | trusted location/network |
| Something you do | behavior pattern |

PKI dan certificate:

| Komponen | Arti |
|---|---|
| CA | certificate authority yang dipercaya |
| Intermediate CA | CA perantara antara root dan leaf certificate |
| Leaf/server certificate | certificate milik service seperti HTTPS/VPN |
| Public key | key yang dibagikan |
| Private key | key rahasia yang harus dilindungi |
| CSR | certificate signing request |
| CRL/OCSP | mekanisme cek certificate dicabut |

Contoh certificate:

```text
Subject: CN=app.example.com
SAN: DNS:app.example.com, DNS:www.example.com
Issuer: Example Intermediate CA
Valid From: 2026-01-01
Valid To: 2027-01-01
Public Key Algorithm: RSA/ECDSA
Signature Algorithm: sha256WithRSAEncryption
```

Field certificate:

| Field | Arti |
|---|---|
| Subject | identitas yang memiliki certificate |
| SAN | hostname/IP yang valid untuk certificate |
| Issuer | CA yang menandatangani |
| Validity | masa berlaku |
| Public key | key publik service |
| Signature | bukti certificate ditandatangani CA |

Certificate problem:

| Gejala | Kemungkinan |
|---|---|
| Browser warning expired | certificate melewati tanggal valid |
| Name mismatch | hostname tidak ada di SAN |
| Untrusted issuer | CA tidak dipercaya client |
| Incomplete chain | intermediate certificate tidak dikirim |
| Revoked | certificate dicabut |

Segmentation:

- Guest network dipisah dari internal.
- IoT/IIoT dipisah karena sering sulit di-hardening.
- SCADA/ICS/OT butuh segmentasi ketat.
- BYOD sebaiknya dipisah dan dikontrol NAC.

Contoh segmentation design:

| Segment | Contoh Subnet | Akses yang Diizinkan |
|---|---|---|
| Users | `10.10.10.0/24` | internet, aplikasi internal tertentu |
| Servers | `10.10.30.0/24` | hanya port aplikasi dari user/LB |
| Management | `10.10.99.0/24` | SSH/HTTPS/SNMP ke perangkat |
| Guest | `10.10.200.0/24` | internet only |
| IoT | `10.10.150.0/24` | hanya controller/update server |
| DMZ | `10.10.80.0/24` | service publik terbatas |

Zero trust:

- Jangan otomatis percaya traffic hanya karena berasal dari network internal.
- Validasi identity, device posture, lokasi, dan context.
- Beri akses minimum ke aplikasi/resource yang dibutuhkan.

Defense in depth:

| Layer Defense | Contoh |
|---|---|
| Physical | locked rack, badge access, CCTV |
| Network | firewall, ACL, segmentation |
| Endpoint | EDR, patching, host firewall |
| Identity | MFA, RBAC, least privilege |
| Application | WAF, secure coding, rate limit |
| Monitoring | SIEM, alerting, log review |

### 4.2 Network Attacks

Attack umum:

| Attack | Dampak | Mitigasi Umum |
|---|---|---|
| DoS/DDoS | service tidak tersedia | rate limit, DDoS protection, filtering |
| VLAN hopping | attacker masuk VLAN lain | disable DTP, set access port, native VLAN khusus |
| MAC flooding | switch CAM table penuh | port security, storm control |
| ARP poisoning/spoofing | traffic diarahkan ke attacker | Dynamic ARP Inspection, static ARP untuk aset penting |
| DNS poisoning/spoofing | nama diarahkan ke IP palsu | DNSSEC, trusted resolver, DoH/DoT jika sesuai |
| Rogue DHCP | client mendapat gateway/DNS palsu | DHCP snooping |
| Rogue AP | wireless palsu | wireless IDS/WIPS, survey, 802.1X |
| Evil twin | AP palsu meniru SSID sah | WPA3/Enterprise, certificate validation |
| On-path attack | attacker berada di jalur komunikasi | TLS, VPN, certificate validation |
| Phishing | user ditipu memberi credential | MFA, awareness, email security |
| Malware | host/network terinfeksi | EDR, segmentation, patching |

ARP poisoning:

```text
Victim mengira MAC attacker adalah gateway.
Gateway mengira MAC attacker adalah victim.
Traffic bisa disadap atau dimodifikasi.
```

VLAN hopping:

- Switch spoofing: attacker mencoba menjadi trunk.
- Double tagging: attacker menyisipkan dua VLAN tag.
- Mitigasi: nonaktifkan DTP, set access port eksplisit, ubah native VLAN, jangan pakai VLAN 1 untuk user.

MAC flooding flow:

```text
1. Attacker mengirim banyak frame dengan source MAC palsu.
2. CAM/MAC address table switch penuh.
3. Switch bisa flood frame karena tidak tahu port tujuan.
4. Attacker mencoba melihat traffic yang seharusnya tidak terlihat.
```

Rogue DHCP flow:

```text
1. Client mengirim DHCP Discover.
2. DHCP server palsu menjawab lebih cepat.
3. Client menerima IP, gateway, atau DNS yang salah.
4. Traffic client diarahkan ke attacker atau network menjadi gagal.
```

DNS poisoning flow:

```text
1. Client bertanya IP untuk nama tertentu.
2. Resolver/cache menerima jawaban palsu.
3. Client diarahkan ke IP attacker.
4. User mengakses service palsu atau traffic disadap.
```

Evil twin flow:

```text
1. Attacker membuat SSID sama seperti Wi-Fi sah.
2. Client terhubung ke AP palsu karena sinyal lebih kuat atau konfigurasi lemah.
3. Attacker mencuri credential atau melakukan on-path attack.
```

DoS vs DDoS:

| Jenis | Arti |
|---|---|
| DoS | satu sumber atau metode membuat service terganggu |
| DDoS | banyak sumber menyerang bersamaan |

Contoh DoS/DDoS:

| Attack | Target |
|---|---|
| Volumetric flood | bandwidth penuh |
| SYN flood | resource TCP/session table |
| Application-layer flood | CPU/backend aplikasi |
| Amplification attack | memakai service pihak ketiga untuk memperbesar traffic |

Wireless attack:

| Attack | Arti | Mitigasi |
|---|---|---|
| Deauthentication | memutus client dari AP | WPA3/802.11w jika didukung |
| Evil twin | SSID palsu | certificate validation, WPA-Enterprise |
| Password cracking | mencoba PSK lemah | password kuat, WPA3-SAE |
| Rogue AP | AP tidak sah di network | WIDS/WIPS, switch port security |

Social engineering:

| Teknik | Arti |
|---|---|
| Phishing | pesan palsu untuk mencuri credential |
| Spear phishing | phishing ditargetkan |
| Vishing | phishing lewat suara/telepon |
| Smishing | phishing lewat SMS/chat |
| Dumpster diving | mencari informasi dari sampah fisik/dokumen |
| Shoulder surfing | mengintip layar/keyboard/credential |
| Tailgating | masuk area fisik mengikuti orang sah |

### 4.3 Security Features, Defense Techniques, and Solutions

Device hardening:

- Disable unused ports/services.
- Change default passwords.
- Pakai SSH/HTTPS, bukan Telnet/HTTP.
- Update firmware.
- Batasi management access.
- Pakai NTP agar log dan certificate valid.
- Simpan backup config sebelum upgrade/change.
- Gunakan banner legal jika diwajibkan policy.

NAC:

| Fitur | Arti |
|---|---|
| 802.1X | port-based authentication |
| Port security | batasi MAC di switch port |
| MAC filtering | allow/deny berdasarkan MAC |

802.1X roles:

| Role | Arti |
|---|---|
| Supplicant | client yang meminta akses |
| Authenticator | switch/AP yang mengontrol port |
| Authentication server | RADIUS server yang memvalidasi user/device |

802.1X flow:

```text
1. Client tersambung ke switch/AP.
2. Switch/AP meminta identitas lewat EAPOL.
3. Client mengirim credential/certificate.
4. Switch/AP meneruskan ke RADIUS.
5. RADIUS menerima/menolak dan bisa memberi VLAN/policy.
6. Port dibuka sesuai authorization.
```

Port security:

| Mode | Arti |
|---|---|
| Static secure MAC | MAC ditulis manual |
| Dynamic secure MAC | switch belajar MAC sampai limit |
| Sticky MAC | switch belajar lalu menyimpan MAC ke config |

Violation action:

| Action | Dampak |
|---|---|
| Protect | drop traffic MAC baru tanpa log detail |
| Restrict | drop dan log/count violation |
| Shutdown | port masuk err-disabled |

ACL:

| Field | Contoh | Arti |
|---|---|---|
| source | `10.10.10.0/24` | asal traffic |
| destination | `192.168.1.10` | tujuan traffic |
| protocol | `tcp` | protocol |
| port | `443` | service |
| action | `permit/deny` | keputusan |

ACL behavior:

- ACL biasanya diproses dari atas ke bawah.
- Rule pertama yang match dipakai.
- Banyak platform punya implicit deny di akhir.
- Rule lebih spesifik sebaiknya ditempatkan sebelum rule umum.

Contoh ACL:

```text
permit tcp 10.10.10.0/24 10.10.30.10 eq 443
permit udp 10.10.10.0/24 10.10.1.10 eq 53
deny ip 10.10.10.0/24 10.10.99.0/24
permit ip 10.10.10.0/24 any
```

Arti:

| Rule | Arti |
|---|---|
| `permit tcp ... eq 443` | user boleh HTTPS ke app server |
| `permit udp ... eq 53` | user boleh DNS ke resolver |
| `deny ip ... management` | user tidak boleh ke network management |
| `permit ip ... any` | user boleh traffic lain sesuai policy |

Firewall type:

| Type | Arti |
|---|---|
| Stateless | mengecek packet satu per satu berdasarkan rule |
| Stateful | melacak connection state |
| Next-generation firewall | inspeksi aplikasi/user/content lebih dalam |
| Web application firewall | fokus HTTP/HTTPS Layer 7 |

Key management:

| Item | Arti |
|---|---|
| Key generation | pembuatan key dengan entropy/algoritma yang kuat |
| Key storage | penyimpanan key di tempat aman seperti HSM/KMS jika tersedia |
| Key rotation | penggantian key berkala atau saat incident |
| Key revocation | mencabut key/certificate yang tidak dipercaya |
| Key escrow | penyimpanan key cadangan dengan kontrol ketat |

Security filtering:

| Kontrol | Fungsi |
|---|---|
| URL filtering | allow/deny berdasarkan URL atau kategori web |
| Content filtering | inspeksi dan blokir content tertentu |
| DNS filtering | blokir domain berbahaya di resolver |

Zones:

| Zone | Arti |
|---|---|
| Trusted | internal/managed |
| Untrusted | internet/unknown |
| Screened subnet | DMZ untuk service publik |

DMZ design:

```text
Internet -> firewall -> DMZ reverse proxy/load balancer -> firewall -> internal app/database
```

Prinsip DMZ:

- Server publik tidak langsung berada di LAN user.
- DMZ hanya boleh membuka port yang dibutuhkan.
- Traffic dari DMZ ke internal harus jauh lebih ketat daripada traffic internal ke DMZ.

Switch security features:

| Feature | Melindungi Dari | Cara Kerja |
|---|---|---|
| DHCP snooping | rogue DHCP | hanya trusted port boleh mengirim DHCP offer |
| Dynamic ARP Inspection | ARP spoofing | validasi ARP dengan DHCP snooping binding |
| IP Source Guard | IP spoofing | membatasi IP source sesuai binding |
| BPDU Guard | switch liar/loop | shutdown port access jika menerima BPDU |
| Root Guard | root bridge tidak sah | mencegah port menjadi root port |
| Storm control | broadcast/multicast flood | membatasi traffic flood |

DHCP snooping trust:

| Port | Trust State | Contoh |
|---|---|---|
| Ke DHCP server/uplink | Trusted | boleh mengirim DHCP offer |
| Ke client/user | Untrusted | DHCP offer diblok |

Wireless security:

| Mode | Catatan |
|---|---|
| Open | tidak ada enkripsi, hindari untuk internal |
| WPA2-Personal | shared password, cocok rumah/small office |
| WPA3-Personal | lebih kuat dari WPA2-PSK |
| WPA2/WPA3-Enterprise | per-user auth via 802.1X/RADIUS |

Remote access security:

| Kontrol | Tujuan |
|---|---|
| MFA | mengurangi risiko password dicuri |
| Device posture | memastikan device memenuhi syarat |
| Split/full tunnel policy | menentukan jalur traffic |
| Least privilege group | membatasi akses setelah VPN connect |
| Logging | audit login, IP, durasi, resource |

Security monitoring:

| Data | Dicari |
|---|---|
| Firewall logs | deny spike, allowed unusual traffic |
| VPN logs | login gagal, lokasi aneh, impossible travel |
| DNS logs | domain malware, tunneling pattern |
| DHCP logs | rogue device, lease spike |
| Wireless logs | deauth, rogue AP, auth failure |
| IDS/IPS alerts | exploit, scanning, C2 traffic |

---

## 5.0 Network Troubleshooting

**Praktik setelah bab ini:** pecahkan masalah dengan urutan bukti, bukan tebakan.

```bash
# Cek DNS resolution.
dig example.com

# Cek route yang dipilih untuk destination.
ip route get 93.184.216.34

# Cek koneksi TCP ke port HTTPS.
nc -vz example.com 443

# Capture traffic ke host tujuan.
sudo tcpdump -i eth0 -nn host 93.184.216.34
```

Catat: layer pertama yang gagal, output yang membuktikan gagal, dan command pembanding dari layer lain.

Bagian ini membahas methodology, physical issues, services, performance, dan tools.

### 5.1 Troubleshooting Methodology

Methodology:

1. Identify the problem.
2. Establish a theory of probable cause.
3. Test the theory.
4. Establish a plan of action.
5. Implement the solution or escalate.
6. Verify full system functionality.
7. Document findings, actions, outcomes, and lessons learned.

Detail identify:

- Gather information.
- Question users.
- Identify symptoms.
- Determine what changed.
- Duplicate the problem if possible.
- Approach multiple problems individually.

Pertanyaan awal yang bagus:

| Pertanyaan | Tujuan |
|---|---|
| Siapa yang terdampak | satu user, satu VLAN, satu site, semua user |
| Kapan mulai terjadi | cari perubahan atau event yang dekat waktunya |
| Apa yang masih berhasil | mempersempit layer/area masalah |
| Apakah ada perubahan terakhir | change network, firewall, DNS, DHCP, server |
| Apakah masalah konsisten | bedakan outage, intermittent, atau performance |
| Dari mana ke mana traffic gagal | source, destination, port, protocol |

Contoh problem statement:

```text
User VLAN 10 di lantai 2 tidak bisa akses https://app.example.com sejak 09:15.
Ping ke gateway berhasil.
DNS resolve berhasil.
TCP 443 ke app timeout.
Perubahan firewall dilakukan pukul 09:00.
```

Field penting:

| Field | Contoh | Arti |
|---|---|---|
| Source | `10.10.10.55` | host asal |
| Destination | `10.10.30.10` | host/service tujuan |
| Protocol | `TCP` | transport |
| Port | `443` | service |
| Symptom | `timeout` | bentuk kegagalan |
| Scope | `VLAN 10 only` | luas dampak |
| Last change | `firewall ACL update` | kandidat penyebab |

Pendekatan OSI:

| Pendekatan | Arti |
|---|---|
| Top-to-bottom | mulai dari aplikasi ke fisik |
| Bottom-to-top | mulai dari fisik ke aplikasi |
| Divide and conquer | mulai dari layer tengah untuk mempersempit |

Evidence per layer:

| Layer | Evidence yang Dicari | Tool/Command |
|---:|---|---|
| 1 | link up/down, speed, optic power | interface status, cable tester |
| 2 | VLAN, MAC table, ARP, STP | switch show commands, ARP table |
| 3 | IP, subnet, gateway, route | `ip route`, `traceroute`, routing table |
| 4 | TCP handshake, port open/closed | `ss`, `netstat`, `nmap`, packet capture |
| 7 | DNS, HTTP status, auth, app log | `dig`, browser/devtools, app logs |

Symptom narrowing:

| Gejala | Kemungkinan Awal |
|---|---|
| Tidak dapat IP | DHCP, VLAN, port, wireless auth |
| Dapat APIPA `169.254.x.x` | DHCP tidak menjawab |
| Bisa gateway, tidak bisa remote subnet | routing, ACL, firewall |
| Bisa IP, nama gagal | DNS |
| TCP timeout | firewall drop, routing blackhole, service down |
| TCP refused/RST | host reachable tapi port tertutup/rejected |
| Lambat semua aplikasi | congestion, loss, duplex, Wi-Fi interference |
| Hanya satu aplikasi lambat | app/server/backend/DNS khusus |

### 5.2 Cabling and Physical Interface Issues

Cable issues:

| Issue | Gejala |
|---|---|
| Incorrect cable | link tidak up atau speed salah |
| Single-mode vs multimode mismatch | fiber link gagal |
| STP/UTP salah | noise/interference |
| Crosstalk | error meningkat |
| Attenuation | signal melemah karena jarak |
| Improper termination | CRC/error/drop |
| TX/RX transposed | fiber link tidak up |

Interface counters:

| Counter | Arti |
|---|---|
| CRC | frame rusak |
| Runts | frame terlalu kecil |
| Giants | frame terlalu besar |
| Drops | packet dibuang |

Port status:

| Status | Arti |
|---|---|
| Error disabled | switch mematikan port karena violation/error |
| Administratively down | port dimatikan manual |
| Suspended | port tidak aktif karena kondisi seperti LACP mismatch |

Speed/duplex:

| Kondisi | Dampak |
|---|---|
| Speed mismatch | link bisa down atau performa buruk |
| Duplex mismatch | collision/error tinggi, throughput rendah |
| Auto-negotiation gagal | speed/duplex tidak sesuai |

Interface counter clues:

| Counter Naik | Dugaan |
|---|---|
| CRC/input errors | kabel buruk, interference, optic lemah |
| Collisions | half-duplex atau duplex mismatch |
| Late collisions | duplex mismatch atau kabel terlalu panjang |
| Output drops | congestion/queue penuh |
| Runts | frame terlalu kecil, collision/bad NIC |
| Giants | MTU/jumbo mismatch atau frame abnormal |

Fiber troubleshooting:

| Gejala | Kemungkinan |
|---|---|
| Link down | TX/RX terbalik, optic salah, fiber putus |
| Link flapping | optic power borderline, konektor kotor |
| Error tinggi | attenuation, dirty connector, bend radius buruk |
| Satu sisi up satu sisi down | transceiver/compatibility atau polarity |

Cable tester result:

| Result | Arti |
|---|---|
| Open | kabel putus |
| Short | pair bersentuhan |
| Split pair | pin benar tapi pair twist salah |
| Reversed pair | urutan pair terbalik |
| Wiremap fail | terminasi salah |

PoE:

| Issue | Arti |
|---|---|
| Power budget exceeded | switch tidak punya daya cukup |
| Incorrect standard | device/switch PoE tidak kompatibel |

PoE troubleshooting:

| Gejala | Kemungkinan |
|---|---|
| AP/IP phone tidak menyala | PoE disabled, budget habis, standard tidak cocok |
| Device boot loop | power tidak cukup |
| Link up tapi device mati | data pair baik, power negotiation gagal |
| Hanya device tertentu gagal | class power device lebih tinggi dari port |

### 5.3 Network Service Issues

Service issues:

| Issue | Dugaan |
|---|---|
| Address pool exhaustion | DHCP scope habis |
| Incorrect gateway | client tidak bisa keluar subnet |
| Duplicate IP | koneksi intermittent |
| Incorrect subnet mask | host salah menentukan local/remote |
| Incorrect DNS | nama gagal resolve |
| ACL block | port/service tidak bisa diakses |
| STP issue | loop atau jalur blocked salah |
| VLAN mismatch | host berada di broadcast domain salah |

Route selection:

- Cek routing table.
- Cek default route.
- Cek longest prefix match.
- Cek administrative distance dan metric.

DHCP issue matrix:

| Gejala | Dugaan | Bukti |
|---|---|---|
| APIPA | DHCP server/relay tidak reachable | tidak ada Offer |
| IP dari subnet salah | VLAN salah atau rogue DHCP | gateway/DNS tidak sesuai IPAM |
| Scope penuh | pool exhaustion | lease active mendekati total pool |
| Duplicate IP | static IP bentrok lease | ARP berubah-ubah |
| Client tidak dapat option | DHCP option salah | gateway/DNS kosong atau salah |

DNS issue matrix:

| Gejala | Dugaan | Bukti |
|---|---|---|
| `NXDOMAIN` | record tidak ada atau nama salah | authoritative jawab tidak ada |
| Timeout | resolver tidak reachable/firewall | query tidak mendapat response |
| Jawaban IP lama | TTL/cache | resolver berbeda memberi jawaban berbeda |
| Hanya internal gagal | split DNS salah | internal resolver berbeda dari external |
| Reverse lookup gagal | PTR/reverse zone belum dibuat | A record ada, PTR tidak ada |

Routing issue matrix:

| Gejala | Dugaan |
|---|---|
| Hop berhenti di gateway | default route/upstream issue |
| Loop di traceroute | routing loop |
| Satu prefix gagal | missing specific route atau ACL |
| Semua internet gagal | default route, NAT, ISP |
| Asymmetric path | return traffic lewat jalur berbeda dan diblok firewall stateful |

NAT/PAT issue matrix:

| Gejala | Kemungkinan |
|---|---|
| Private IP keluar tanpa translate | NAT rule tidak match |
| Koneksi keluar timeout | PAT pool/port exhaustion atau upstream block |
| Inbound service gagal | port forward/DNAT/firewall rule salah |
| Hanya beberapa app gagal | ALG/inspection atau port khusus |

Firewall issue matrix:

| Gejala | Kemungkinan |
|---|---|
| TCP timeout | firewall drop silently |
| TCP RST/refused | firewall reject atau service tertutup |
| Ping gagal tapi app bisa | ICMP diblok, bukan selalu routing gagal |
| App sebagian jalan | port tambahan belum dibuka |
| Return traffic drop | state table/asymmetric routing |

### 5.4 Performance Issues

Performance metrics:

| Metric | Arti |
|---|---|
| Bandwidth | kapasitas link |
| Throughput | traffic aktual berhasil lewat |
| Latency | delay |
| Jitter | variasi delay |
| Packet loss | packet hilang |
| Congestion | link/perangkat terlalu penuh |
| Bottleneck | titik pembatas performa |

Cara membaca metric:

| Metric | Interpretasi |
|---|---|
| Bandwidth tinggi tapi throughput rendah | loss, TCP window, congestion, server limit |
| Latency tinggi | jarak, queue, routing path, overloaded device |
| Jitter tinggi | queue tidak stabil, Wi-Fi, WAN congestion |
| Packet loss kecil tapi terasa | TCP retransmission bisa menurunkan throughput besar |
| Utilization 95%+ | link hampir penuh, butuh QoS/upgrade |

TCP performance:

| Gejala Capture | Arti |
|---|---|
| Retransmission | packet hilang atau ACK terlambat |
| Duplicate ACK | receiver melihat gap sequence |
| Zero window | receiver buffer penuh |
| Window scaling issue | throughput TCP terbatas |
| Out-of-order | path/load balancing membuat urutan packet berubah |

QoS:

| Konsep | Arti |
|---|---|
| Classification | mengenali jenis traffic |
| Marking | memberi tanda seperti DSCP |
| Queuing | menentukan antrian traffic |
| Shaping | menahan traffic agar sesuai rate |
| Policing | drop/remark traffic yang melebihi rate |

QoS use case:

| Traffic | Kebutuhan |
|---|---|
| Voice | latency/jitter rendah |
| Video conference | bandwidth dan jitter stabil |
| Backup | bisa diperlambat saat jam kerja |
| Interactive app | latency rendah |
| Bulk download | prioritas rendah |

Wireless performance:

| Issue | Dampak |
|---|---|
| Interference | throughput turun |
| Channel overlap | client saling mengganggu |
| Signal loss | disconnect/slow |
| Insufficient coverage | dead zone |
| Client disassociation | client sering putus |
| Roaming misconfiguration | pindah AP tidak mulus |

Wireless root cause clues:

| Clue | Dugaan |
|---|---|
| RSSI lemah | AP terlalu jauh, obstacle, power rendah |
| SNR rendah | noise/interference tinggi |
| Channel utilization tinggi | terlalu banyak client/AP di channel sama |
| Banyak retry | interference atau signal buruk |
| Roaming lambat | AP spacing, power, 802.11k/v/r config |
| Hanya 2.4 GHz lambat | channel overlap atau device legacy |

MTU/path MTU:

| Gejala | Kemungkinan |
|---|---|
| Ping kecil sukses, transfer besar gagal | MTU/path MTU issue |
| VPN aplikasi tertentu hang | overhead tunnel membuat packet terlalu besar |
| Fragmentation needed | DF bit dan path MTU tidak sesuai |

### 5.5 Tools and Protocols

Software tools:

```bash
# Menguji reachability dan latency dasar.
ping 8.8.8.8

# Melacak jalur packet ke tujuan di Linux/macOS.
traceroute example.com

# Melacak jalur packet ke tujuan di Windows.
tracert example.com

# Query DNS sederhana.
nslookup example.com

# Query DNS detail.
dig example.com

# Menampilkan interface dan IP di Linux.
ip addr

# Menampilkan interface dengan tool legacy jika tersedia.
ifconfig

# Menampilkan interface dan IP di Windows.
ipconfig /all

# Melihat ARP/neighbor table.
arp -a

# Melihat koneksi dan listening port.
netstat -ano

# Melihat listening socket di Linux.
ss -tulpn

# Melihat routing table di Linux.
ip route

# Melihat neighbor table di Linux.
ip neigh

# Packet capture CLI.
tcpdump -i eth0

# Scan port host target.
nmap 192.168.1.10

# Menguji bandwidth antar host.
iperf3 -c 192.168.1.20

# Melihat path dan loss secara kontinu jika tersedia.
mtr example.com
```

Tool kategori:

| Tool | Fungsi |
|---|---|
| Protocol analyzer | analisis packet detail, misalnya Wireshark/tcpdump |
| Command line tools | uji cepat reachability, DNS, route, socket |
| Nmap | discovery host dan port/service |
| LLDP/CDP | melihat neighbor Layer 2 |
| Speed tester | mengukur throughput ke endpoint tertentu |

`ping` output yang dibaca:

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.4 ms
```

Field:

| Field | Arti |
|---|---|
| bytes | ukuran payload ICMP |
| icmp_seq | urutan packet |
| ttl | sisa hop limit |
| time | round-trip time |
| packet loss | persentase packet tidak mendapat reply |

Interpretasi `ping`:

| Hasil | Arti |
|---|---|
| reply stabil | reachability dasar baik |
| request timeout | host/firewall/routing bisa bermasalah |
| latency naik turun | congestion, Wi-Fi, WAN issue |
| packet loss | link/perangkat/path bermasalah |
| destination unreachable | router/host memberi tahu route/port/network gagal |

`traceroute`/`tracert`:

```text
1  192.168.1.1     1 ms
2  10.0.0.1        5 ms
3  * * *
4  203.0.113.10   30 ms
```

Field:

| Field | Arti |
|---|---|
| hop number | urutan router |
| hostname/IP | router yang merespons TTL expired |
| latency per probe | waktu reply tiap probe |
| `*` | tidak ada reply, bisa filter ICMP/TTL atau drop |

Catatan:

- Hop `*` tidak selalu berarti traffic aplikasi gagal.
- Yang penting adalah apakah tujuan akhir tercapai.
- Path pergi dan path pulang bisa berbeda.

`dig` output yang dibaca:

```text
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
;; ANSWER SECTION:
www.example.com. 300 IN A 203.0.113.10
```

Field:

| Field | Arti |
|---|---|
| status | hasil query seperti `NOERROR`, `NXDOMAIN`, `SERVFAIL` |
| ANSWER SECTION | jawaban record |
| TTL | sisa waktu cache |
| record type | A, AAAA, CNAME, MX, TXT, dan lain-lain |
| server | resolver yang menjawab |

Status DNS:

| Status | Arti |
|---|---|
| NOERROR | query berhasil |
| NXDOMAIN | nama tidak ada |
| SERVFAIL | server gagal memproses |
| REFUSED | server menolak query |

`ipconfig /all` field penting:

| Field | Arti |
|---|---|
| IPv4 Address | IP client |
| Subnet Mask | mask subnet |
| Default Gateway | gateway keluar subnet |
| DHCP Server | server pemberi lease |
| DNS Servers | resolver yang dipakai |
| Lease Obtained/Expires | waktu lease DHCP |

`nmap` state:

| State | Arti |
|---|---|
| open | service menerima koneksi |
| closed | host reachable tapi port tertutup |
| filtered | nmap tidak bisa memastikan karena filter/firewall |
| open/filtered | tidak ada response jelas, umum pada UDP |

`tcpdump` sample:

```text
10:10:01.123456 IP 192.168.1.10.51544 > 93.184.216.34.443: Flags [S], seq 1000
10:10:01.150000 IP 93.184.216.34.443 > 192.168.1.10.51544: Flags [S.], ack 1001
10:10:01.151000 IP 192.168.1.10.51544 > 93.184.216.34.443: Flags [.], ack 2001
```

Field:

| Field | Arti |
|---|---|
| timestamp | waktu packet terlihat |
| IP | protocol Layer 3 |
| source.ip.port | asal traffic |
| destination.ip.port | tujuan traffic |
| Flags `[S]` | SYN |
| Flags `[S.]` | SYN-ACK |
| Flags `[.]` | ACK |

Capture filter vs display filter:

| Filter | Dipakai Di | Arti |
|---|---|---|
| Capture filter | sebelum packet disimpan | mengurangi packet yang ditangkap |
| Display filter | setelah packet ditangkap | menyaring tampilan analisis |

Hardware tools:

| Tool | Fungsi |
|---|---|
| Toner/probe | menelusuri kabel |
| Cable tester | menguji wiring/continuity |
| Tap | mengambil copy traffic |
| Wi-Fi analyzer | melihat channel/signal/interference |
| Visual fault locator | mencari masalah fiber |

Device commands:

```text
show mac-address-table
show route
show interface
show config
show arp
show vlan
show power
```

Interpretasi cepat:

| Command | Dicari |
|---|---|
| `show mac-address-table` | MAC belajar di port/VLAN mana |
| `show route` | route dan next hop |
| `show interface` | status, speed, duplex, error counter |
| `show config` | konfigurasi aktif |
| `show arp` | mapping IP ke MAC |
| `show vlan` | VLAN dan port membership |
| `show power` | status PoE/power |

---

## 6.0 Senior Deep Dive

**Praktik setelah bab ini:** baca state kernel, NIC, TCP, DNS, dan packet capture secara bersamaan.

```bash
# Melihat detail TCP socket, timer, dan congestion info.
ss -ti

# Melihat fitur offload NIC.
ethtool -k eth0

# Capture traffic ke file pcap untuk analisis lanjutan.
sudo tcpdump -i eth0 -nn -w network-deep-dive.pcap
```

Catat: retransmission, RTT, congestion state, offload yang bisa memengaruhi capture, dan bukti packet-level yang mendukung kesimpulan.

Bagian ini untuk membaca network seperti operator senior: bukan hanya "bisa ping atau tidak", tetapi packet lewat jalur mana, state apa yang dibuat, cache mana yang dipakai, counter mana yang naik, dan bukti apa yang cukup kuat untuk menyatakan root cause.

### 6.1 Packet Path dan Kernel Networking

Packet path adalah jalur packet dari NIC sampai aplikasi, atau dari aplikasi sampai NIC. Untuk sysadmin/network engineer, ini penting karena firewall, NAT, routing, QoS, packet capture, dan offload tidak selalu terjadi di titik yang sama.

RX path pada Linux host:

```text
Wire/fiber/radio
-> NIC receive queue
-> DMA ke memory
-> interrupt / NAPI poll
-> driver membuat skb
-> XDP jika aktif
-> tc ingress jika aktif
-> netfilter PREROUTING
-> routing decision
-> INPUT hook
-> socket receive queue
-> application read()
```

Forwarding path pada Linux router/firewall:

```text
NIC RX
-> driver/skb
-> PREROUTING
-> routing decision
-> FORWARD
-> POSTROUTING
-> qdisc egress
-> NIC TX
```

Outbound path dari aplikasi lokal:

```text
application write()
-> socket send buffer
-> routing decision
-> OUTPUT
-> POSTROUTING
-> qdisc egress
-> NIC TX
```

Netfilter hook:

| Hook | Kapan Terjadi | Contoh Fungsi |
|---|---|---|
| PREROUTING | sebelum routing decision | DNAT, mangle, raw tracking decision |
| INPUT | packet untuk host lokal | local firewall |
| FORWARD | packet yang diteruskan | router/firewall forwarding policy |
| OUTPUT | packet dibuat host lokal | local outbound firewall |
| POSTROUTING | setelah routing decision | SNAT/MASQUERADE, mangle egress |

NAT timing:

| NAT | Hook Umum | Alasan |
|---|---|---|
| DNAT | PREROUTING | destination perlu diubah sebelum route lookup |
| SNAT/MASQUERADE | POSTROUTING | source diubah setelah egress interface diketahui |

Conntrack:

| State | Arti |
|---|---|
| NEW | koneksi baru terlihat |
| ESTABLISHED | packet bagian dari koneksi yang sudah valid |
| RELATED | koneksi baru terkait koneksi lain, misalnya FTP data atau ICMP error |
| INVALID | packet tidak cocok state yang valid |
| UNTRACKED | sengaja tidak dilacak conntrack |

Hal penting conntrack:

- NAT mapping biasanya dibuat dari packet pertama koneksi.
- Rule NAT yang diubah tidak selalu memengaruhi koneksi lama sampai state lama hilang.
- Conntrack table penuh bisa membuat koneksi baru gagal walau firewall rule benar.
- Asymmetric routing bisa membuat satu arah traffic tidak terlihat firewall stateful.

NIC offload:

| Feature | Fungsi | Efek Saat Capture |
|---|---|---|
| Checksum offload | checksum dihitung NIC | tcpdump bisa terlihat checksum salah di host lokal |
| TSO/GSO | segment besar dipecah nanti | packet capture lokal bisa terlihat lebih besar dari MTU |
| GRO/LRO | packet kecil digabung saat RX | capture lokal bisa terlihat tidak sama dengan wire |
| RSS | flow dibagi ke beberapa RX queue/CPU | membantu throughput multi-core |
| VLAN offload | tag VLAN diproses NIC | tag bisa tidak terlihat di capture tertentu |

Queue dan buffer:

| Komponen | Arti |
|---|---|
| RX ring | buffer descriptor untuk packet masuk di NIC |
| TX ring | buffer descriptor untuk packet keluar di NIC |
| qdisc | queueing discipline egress Linux |
| socket buffer | buffer per socket di kernel |
| backlog | antrian packet/koneksi sebelum aplikasi mengambilnya |

Command observasi packet path:

```bash
# Melihat counter interface termasuk error/drop di Linux.
ip -s link show dev eth0

# Melihat fitur offload NIC.
ethtool -k eth0

# Melihat statistik driver/NIC jika didukung.
ethtool -S eth0

# Melihat queue channel NIC.
ethtool -l eth0

# Melihat qdisc egress.
tc qdisc show dev eth0

# Melihat ruleset nftables.
nft list ruleset

# Melihat conntrack entry aktif jika tool tersedia.
conntrack -L

# Melihat ring buffer NIC.
ethtool -g eth0
```

Cara membaca gejala low-level:

| Gejala | Dugaan |
|---|---|
| `rx_dropped` naik | ring penuh, CPU tidak sempat proses, driver/NIC issue |
| `tx_dropped` naik | egress queue penuh atau shaping/policing |
| checksum error hanya di capture lokal | kemungkinan checksum offload normal |
| conntrack count mendekati max | risiko koneksi baru drop |
| capture ingress ada, egress tidak ada | firewall/routing/conntrack/qdisc perlu dicek |
| egress ada, reply tidak ada | remote path, return route, firewall lawan, NAT |

### 6.2 TCP State, Congestion, dan Socket Tuning

TCP adalah byte stream yang stateful. Saat troubleshooting senior, pertanyaannya bukan hanya port terbuka, tetapi state koneksi, queue, retransmission, window, dan siapa yang menutup koneksi.

TCP state umum:

| State | Arti | Catatan Troubleshooting |
|---|---|---|
| LISTEN | service menunggu koneksi | cek bind address dan firewall |
| SYN-SENT | client sudah kirim SYN | jika lama, SYN-ACK tidak kembali |
| SYN-RECEIVED | server menerima SYN dan kirim SYN-ACK | bisa banyak saat SYN flood/backlog penuh |
| ESTABLISHED | koneksi aktif | cek throughput, window, retransmission |
| FIN-WAIT-1 | host lokal mulai close | menunggu ACK/FIN |
| FIN-WAIT-2 | FIN lokal sudah ACK | remote belum close |
| CLOSE-WAIT | remote sudah close, aplikasi lokal belum close socket | sering indikasi aplikasi tidak menutup koneksi |
| LAST-ACK | local sudah close setelah CLOSE-WAIT | menunggu ACK final |
| TIME-WAIT | koneksi ditahan setelah close | normal, melindungi dari packet lama |
| CLOSED | tidak ada koneksi | final state |

Backlog saat server menerima koneksi:

| Queue | Arti |
|---|---|
| SYN backlog | antrian half-open connection sebelum handshake selesai |
| Accept queue | koneksi established yang belum diambil aplikasi dengan `accept()` |
| Listen backlog | batas antrian yang diminta aplikasi saat listen |

Jika backlog penuh:

- Client bisa melihat timeout atau reset.
- Server bisa mencatat SYN drop atau listen overflow.
- Load balancer health check bisa gagal intermittent.
- Menambah worker aplikasi kadang lebih tepat daripada hanya menaikkan sysctl.

TCP congestion dan flow control:

| Konsep | Arti |
|---|---|
| cwnd | congestion window, batas sender berdasarkan kondisi network |
| rwnd | receive window, batas berdasarkan kemampuan receiver |
| slow start | cwnd naik cepat di awal koneksi |
| congestion avoidance | cwnd naik lebih hati-hati setelah threshold |
| fast retransmit | retransmit setelah duplicate ACK |
| RTO | retransmission timeout |
| SACK | receiver bisa memberitahu segment mana yang sudah diterima |

Gejala TCP low-level:

| Gejala | Interpretasi |
|---|---|
| Banyak `SYN-SENT` | outbound path/reply/firewall/server bermasalah |
| Banyak `SYN-RECEIVED` | SYN flood, return path, atau backlog |
| Banyak `CLOSE-WAIT` | aplikasi lokal tidak close socket |
| Banyak `TIME-WAIT` | koneksi pendek sangat banyak, sering normal di client/proxy |
| Zero window | receiver lambat membaca buffer |
| Retransmission tinggi | loss, congestion, duplex, Wi-Fi, policing |
| RST dari server | service menolak, aplikasi close paksa, atau firewall reject |

Socket command:

```bash
# Ringkasan socket TCP/UDP.
ss -s

# Melihat TCP socket dengan detail timer dan congestion info.
ss -tin

# Melihat service yang listen beserta process.
ss -tulpn

# Melihat koneksi dalam state SYN-SENT.
ss -tan state syn-sent

# Melihat koneksi dalam state CLOSE-WAIT.
ss -tan state close-wait

# Melihat koneksi dalam state TIME-WAIT.
ss -tan state time-wait

# Melihat range ephemeral port.
sysctl net.ipv4.ip_local_port_range

# Melihat batas backlog umum.
sysctl net.core.somaxconn

# Melihat batas SYN backlog.
sysctl net.ipv4.tcp_max_syn_backlog
```

Tuning yang harus dipahami, bukan asal dinaikkan:

| Parameter | Fungsi | Risiko Jika Salah |
|---|---|---|
| `net.core.somaxconn` | batas accept queue kernel | tidak membantu jika aplikasi lambat |
| `net.ipv4.tcp_max_syn_backlog` | batas SYN backlog | bisa menutupi gejala attack tanpa mitigasi |
| `net.ipv4.ip_local_port_range` | range ephemeral port | port exhaustion jika terlalu kecil |
| `net.ipv4.tcp_fin_timeout` | durasi FIN-WAIT-2 | terlalu agresif bisa memutus koneksi valid |
| socket receive/send buffer | kapasitas buffer TCP | memory pressure jika terlalu besar |

### 6.3 DNS Delegation, Caching, dan Failure Mode

DNS senior-level berarti memahami authority, delegation, cache, TTL, negative answer, transport, dan perbedaan masalah resolver vs authoritative server.

DNS resolution detail:

```text
stub resolver di client
-> recursive resolver
-> root nameserver
-> TLD nameserver
-> authoritative nameserver
-> answer dicache oleh recursive resolver
-> answer dikembalikan ke client
```

Delegation:

| Komponen | Arti |
|---|---|
| Parent zone | zone yang menunjuk NS child zone |
| Child zone | zone yang menerima delegation |
| NS record | menyatakan authoritative nameserver |
| Glue record | IP nameserver jika nameserver berada di dalam child zone |
| SOA record | metadata authority, serial, refresh, retry, expire |

Contoh delegation:

```text
example.com.      NS   ns1.example.com.
ns1.example.com.  A    203.0.113.53
```

Glue dibutuhkan karena `ns1.example.com` berada di dalam domain `example.com`. Tanpa glue, resolver bisa masuk circular dependency.

SOA field penting:

| Field | Arti |
|---|---|
| MNAME | primary authoritative server |
| RNAME | email admin zone |
| Serial | versi zone untuk transfer/replication |
| Refresh | interval secondary cek update |
| Retry | interval retry jika refresh gagal |
| Expire | kapan secondary berhenti menjawab jika primary tidak reachable |
| Minimum/negative TTL | cache untuk jawaban negatif |

Zone transfer:

| Type | Arti |
|---|---|
| AXFR | transfer seluruh zone |
| IXFR | transfer perubahan incremental |
| Notify | primary memberi tahu secondary ada update |

Failure mode DNS:

| Gejala | Root Cause Kandidat |
|---|---|
| `NXDOMAIN` | nama benar-benar tidak ada, typo, atau delegation salah |
| `SERVFAIL` | DNSSEC fail, authoritative error, resolver gagal recurse |
| `REFUSED` | server menolak recursion/query dari client |
| Timeout | firewall, routing, server down, UDP fragment drop |
| Jawaban berbeda antar resolver | cache, split horizon, propagation, geo/anycast |
| Zone secondary stale | transfer gagal, serial tidak naik, firewall TCP 53 |

DNS command senior:

```bash
# Trace delegation dari root sampai authoritative.
dig +trace www.example.com

# Query authoritative nameserver langsung.
dig @ns1.example.com www.example.com A

# Melihat SOA record.
dig example.com SOA

# Melihat NS record.
dig example.com NS

# Menguji DNS lewat TCP.
dig +tcp example.com A

# Melihat reverse lookup.
dig -x 203.0.113.10

# Melihat DNSSEC validation detail jika tersedia.
dig +dnssec example.com A
```

DNSSEC ringkas:

| Record | Fungsi |
|---|---|
| DNSKEY | public key zone |
| DS | hash key child zone di parent zone |
| RRSIG | signature record set |
| NSEC/NSEC3 | bukti record tidak ada |

EDNS dan ukuran response:

- EDNS0 memungkinkan DNS UDP response lebih besar dari limit lama 512 bytes.
- Jika response besar dan fragment UDP diblok, DNS bisa timeout.
- TCP 53 harus diizinkan untuk zone transfer dan fallback response besar.

### 6.4 Routing, ARP/ND, dan Failure Domain

Routing tidak cukup dibaca dari satu table. Perlu tahu route lookup, neighbor resolution, policy routing, asymmetric path, dan domain kegagalan.

Route lookup order umum:

```text
local route
-> policy rule
-> route table
-> longest prefix match
-> next hop
-> neighbor resolution
-> egress interface
```

Longest prefix match:

| Route | Cocok Untuk | Prioritas |
|---|---|---|
| `10.10.10.55/32` | satu host | paling spesifik |
| `10.10.10.0/24` | satu subnet | lebih spesifik dari `/16` |
| `10.10.0.0/16` | banyak subnet | lebih umum |
| `0.0.0.0/0` | default route | paling umum |

Policy-based routing:

| Match | Aksi |
|---|---|
| source subnet tertentu | pakai route table khusus |
| fwmark tertentu | pakai next hop tertentu |
| traffic VPN | pakai tunnel interface |
| management traffic | pakai OOB atau VRF management |

ARP/ND lifecycle:

| State | Arti |
|---|---|
| INCOMPLETE | sedang mencari MAC |
| REACHABLE | neighbor valid |
| STALE | entry lama, bisa dipakai tapi perlu verifikasi |
| DELAY/PROBE | kernel sedang memverifikasi neighbor |
| FAILED | neighbor resolution gagal |

Failure domain:

| Domain | Contoh Failure | Dampak |
|---|---|---|
| Broadcast domain | loop, ARP flood, rogue DHCP | satu VLAN terganggu |
| Failure domain routing | route salah, adjacency down | prefix/site tertentu terganggu |
| Security zone | ACL/firewall salah | aplikasi atau segment tertentu blocked |
| Control plane | STP/OSPF/BGP flapping | jalur berubah atau network unstable |
| Data plane | ASIC/NIC/queue/drop | control terlihat up, traffic tetap drop |
| Management plane | SSH/SNMP/API down | traffic user bisa tetap jalan |

Routing command:

```bash
# Melihat route untuk destination tertentu.
ip route get 10.10.30.10

# Melihat semua policy rule.
ip rule show

# Melihat route table utama.
ip route show table main

# Melihat neighbor table.
ip neigh show

# Menghapus satu neighbor entry untuk memaksa resolusi ulang.
ip neigh flush 10.10.10.1 dev eth0

# Trace path dengan TCP SYN ke port HTTPS jika tersedia.
traceroute -T -p 443 example.com

# Trace path dengan MTR.
mtr example.com
```

Asymmetric routing:

```text
Client -> Firewall A -> Server
Server -> Firewall B -> Client
```

Dampak:

- Firewall stateful A melihat request.
- Firewall B melihat reply tanpa state.
- Reply bisa didrop sebagai invalid.
- Packet capture satu sisi terlihat "benar", tetapi end-to-end tetap gagal.

ECMP dan hashing:

| Konsep | Arti |
|---|---|
| ECMP | beberapa next hop dengan cost sama |
| Flow hashing | packet flow yang sama diarahkan ke path sama |
| Polarization | hash membuat banyak flow menumpuk di link tertentu |
| Reordering | packet keluar urutan jika hashing/flow tidak konsisten |

### 6.5 Observability, Packet Capture, dan Incident Workflow

Observability network menggabungkan packet, flow, counters, logs, metrics, dan change timeline. Satu sumber data jarang cukup.

Sumber bukti:

| Bukti | Menjawab |
|---|---|
| Packet capture | packet benar-benar terlihat atau tidak |
| Flow data | siapa bicara dengan siapa dan berapa banyak |
| Interface counters | drop/error/congestion di port mana |
| Firewall logs | rule mana yang permit/deny |
| Conntrack/session table | state koneksi dibuat atau tidak |
| DNS logs | nama resolve ke mana |
| Application logs | request sampai aplikasi atau tidak |
| Change log | apa yang berubah sebelum incident |

Capture point:

| Lokasi Capture | Bisa Membuktikan |
|---|---|
| Client | request dibuat dan reply diterima/tidak |
| Default gateway | traffic keluar VLAN |
| Firewall ingress | traffic sampai zone/security boundary |
| Firewall egress | traffic lolos policy/NAT |
| Server | request sampai workload |
| SPAN/TAP | traffic di segment tertentu tanpa host offload bias |

Capture filter penting:

```bash
# Capture traffic host tertentu.
tcpdump -i eth0 host 10.10.30.10

# Capture traffic TCP port 443.
tcpdump -i eth0 tcp port 443

# Capture DNS UDP/TCP port 53.
tcpdump -i eth0 port 53

# Capture ARP.
tcpdump -i eth0 arp

# Capture tanpa reverse DNS agar output cepat.
tcpdump -n -i eth0 host 10.10.30.10

# Simpan capture ke file pcap.
tcpdump -n -i eth0 host 10.10.30.10 -w issue.pcap
```

Incident timeline:

| Waktu | Fakta |
|---|---|
| 09:00 | firewall policy change diterapkan |
| 09:05 | alert synthetic check HTTPS gagal |
| 09:07 | user VLAN 10 melapor timeout |
| 09:10 | capture client melihat SYN retransmission |
| 09:12 | firewall log menunjukkan deny TCP 443 |
| 09:15 | rollback rule dilakukan |
| 09:17 | synthetic check pulih |

RCA format:

| Bagian | Isi |
|---|---|
| Impact | siapa/apa yang terdampak |
| Detection | bagaimana incident diketahui |
| Timeline | urutan fakta berdasarkan waktu |
| Root cause | penyebab teknis utama |
| Contributing factor | faktor pendukung |
| Resolution | tindakan pemulihan |
| Prevention | kontrol agar tidak terulang |
| Evidence | capture, log, counter, change ID |

### 6.6 Performance Engineering dan Capacity Planning

Performance senior-level bukan hanya "bandwidth kurang". Harus dipisahkan antara capacity, utilization, latency, queueing, packet loss, application limit, dan desain path.

Capacity vs performance:

| Konsep | Arti |
|---|---|
| Capacity | batas maksimum resource |
| Utilization | resource yang sedang dipakai |
| Saturation | resource mendekati/menyentuh batas |
| Throughput | data berhasil dipindahkan per waktu |
| Latency | waktu tempuh |
| Queueing delay | delay karena antrian |
| Packet loss | packet hilang/drop |

Little's Law untuk network operations:

```text
Queue length = arrival rate x wait time
```

Artinya, saat arrival rate mendekati service rate, queue naik dan latency bisa melonjak sebelum link terlihat benar-benar 100%.

Bandwidth-delay product:

```text
BDP = bandwidth x RTT
```

Contoh:

| Link | RTT | BDP |
|---|---:|---:|
| 1 Gbps | 1 ms | sekitar 125 KB |
| 1 Gbps | 80 ms | sekitar 10 MB |
| 10 Gbps | 80 ms | sekitar 100 MB |

Implikasi:

- Long fat network butuh TCP window cukup besar.
- Throughput single TCP flow bisa rendah jika window scaling/buffer tidak cukup.
- Banyak flow parallel bisa memenuhi link walau satu flow lambat.

Capacity planning signal:

| Signal | Arti |
|---|---|
| 95th percentile utilization tinggi | link sering mendekati penuh |
| output drops naik saat jam sibuk | queue egress penuh |
| latency naik saat utilization naik | congestion/queueing |
| retransmission naik | loss memukul TCP |
| CPU control plane tinggi | routing/ARP/SNMP/logging bisa terganggu |
| memory/conntrack mendekati max | koneksi baru berisiko gagal |

Performance command:

```bash
# Menguji throughput TCP ke server iperf3.
iperf3 -c 10.10.30.10

# Menguji throughput dengan beberapa parallel stream.
iperf3 -c 10.10.30.10 -P 4

# Menguji UDP dengan bandwidth tertentu.
iperf3 -u -b 50M -c 10.10.30.10

# Melihat retransmission dan TCP detail.
ss -tin

# Melihat statistik TCP kernel.
netstat -s

# Melihat interface counter ringkas.
ip -s link show

# Melihat qdisc dan drop.
tc -s qdisc show dev eth0
```

QoS senior notes:

| Mekanisme | Kegunaan | Risiko |
|---|---|---|
| Classification | mengenali traffic | salah klasifikasi membuat policy tidak efektif |
| Marking DSCP | memberi prioritas end-to-end | bisa dihapus provider |
| Queuing | memberi antrian berbeda | salah queue bisa starvation |
| Shaping | merapikan rate sebelum bottleneck | menambah latency jika terlalu ketat |
| Policing | membuang traffic di atas rate | bisa menyebabkan TCP retransmission |
| WRED | drop lebih awal sebelum queue penuh | perlu tuning hati-hati |

Checklist performance incident:

1. Tentukan apakah masalah latency, loss, throughput, jitter, atau aplikasi.
2. Bandingkan jam bermasalah dengan baseline normal.
3. Cek interface utilization, drop, error, queue.
4. Cek TCP retransmission/window/zero window.
5. Cek apakah satu flow atau semua flow terdampak.
6. Cek path berbeda: LAN, WAN, VPN, internet, cloud.
7. Validasi setelah perubahan dengan metric yang sama.

---
