# HTTP

Sumber standar utama:

- HTTP Semantics: https://www.rfc-editor.org/rfc/rfc9110
- HTTP/1.1: https://www.rfc-editor.org/rfc/rfc9112
- HTTP/2: https://www.rfc-editor.org/rfc/rfc9113
- HTTP/3: https://www.rfc-editor.org/rfc/rfc9114
- HTTP Field Name Registry: https://www.iana.org/assignments/http-fields/http-fields.xhtml
- HTTP Status Code Registry: https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml
- TLS 1.3: https://www.rfc-editor.org/rfc/rfc8446
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/

Area utama:

| Area | Cakupan |
|---|---|
| Fundamentals | URL, origin, request, response, method, status code |
| HTTP/1.1 Low-Level | request line, CRLF, Host, Content-Length, chunked body, raw request |
| Headers, Cookies, and Auth | header scope, cookie attribute, Authorization, CORS, content negotiation |
| Caching, Proxy, and TLS | Cache-Control, ETag, forward/reverse proxy, SNI, certificate |
| HTTP Versions | HTTP/1.1, HTTP/2, HTTP/3, ALPN, transport behavior |
| Web Security | parser boundary, security header, request smuggling, SSRF, cache poisoning |

## Daftar Isi

- [1.0 HTTP Fundamentals](#10-http-fundamentals)
  - [1.1 HTTP Sebagai Application Layer Protocol](#11-http-sebagai-application-layer-protocol)
  - [1.2 URL, Origin, Authority, dan Path](#12-url-origin-authority-dan-path)
  - [1.3 Request, Response, Header, dan Body](#13-request-response-header-dan-body)
  - [1.4 HTTP Methods](#14-http-methods)
  - [1.5 Status Codes](#15-status-codes)
- [2.0 HTTP/1.1 Low-Level](#20-http11-low-level)
  - [2.1 Request Line dan CRLF](#21-request-line-dan-crlf)
  - [2.2 Host Header dan Virtual Hosting](#22-host-header-dan-virtual-hosting)
  - [2.3 Content-Length, Transfer-Encoding, dan Chunked Body](#23-content-length-transfer-encoding-dan-chunked-body)
  - [2.4 Keep-Alive, Connection Close, dan Pipelining](#24-keep-alive-connection-close-dan-pipelining)
  - [2.5 Raw HTTP dengan Telnet, Netcat, dan OpenSSL](#25-raw-http-dengan-telnet-netcat-dan-openssl)
  - [2.6 Request Target Forms](#26-request-target-forms)
  - [2.7 Hop-by-Hop dan End-to-End Headers](#27-hop-by-hop-dan-end-to-end-headers)
- [3.0 Headers, Cookies, and Auth](#30-headers-cookies-and-auth)
  - [3.1 Header Penting](#31-header-penting)
  - [3.2 Cookies and Session](#32-cookies-and-session)
  - [3.3 Authorization Header](#33-authorization-header)
  - [3.4 CORS, Origin, and Browser Enforcement](#34-cors-origin-and-browser-enforcement)
  - [3.5 Content Negotiation and Compression](#35-content-negotiation-and-compression)
  - [3.6 Multipart Form Upload](#36-multipart-form-upload)
- [4.0 Caching, Proxy, and TLS](#40-caching-proxy-and-tls)
  - [4.1 Cache-Control, ETag, and Conditional Request](#41-cache-control-etag-and-conditional-request)
  - [4.2 Forward Proxy, Reverse Proxy, and Load Balancer](#42-forward-proxy-reverse-proxy-and-load-balancer)
  - [4.3 HTTPS, TLS, SNI, and Certificate](#43-https-tls-sni-and-certificate)
- [5.0 HTTP Versions](#50-http-versions)
  - [5.1 HTTP/1.1](#51-http11)
  - [5.2 HTTP/2](#52-http2)
  - [5.3 HTTP/3](#53-http3)
  - [5.4 ALPN and Version Negotiation](#54-alpn-and-version-negotiation)
- [6.0 Web Security Notes](#60-web-security-notes)
  - [6.1 Request Parsing and Trust Boundaries](#61-request-parsing-and-trust-boundaries)
  - [6.2 Header-Based Security Controls](#62-header-based-security-controls)
  - [6.3 Request Smuggling Concepts](#63-request-smuggling-concepts)
  - [6.4 SSRF, Host Header, and URL Confusion](#64-ssrf-host-header-and-url-confusion)
  - [6.5 Cache Poisoning and Web Cache Deception](#65-cache-poisoning-and-web-cache-deception)

---

## 1.0 HTTP Fundamentals

**Fokus teknis:** lihat HTTP sebagai request, response, header, body, dan status code yang bisa dibaca mentah.

```bash
# Melihat request/response HTTP yang dibuat curl secara verbose.
curl -v http://example.com/

# Mengambil hanya response header agar fokus ke status line dan metadata.
curl -I http://example.com/
```

Aspek teknis: request line, status line, header penting, body, redirect, dan apakah server menutup koneksi.

### 1.1 HTTP Sebagai Application Layer Protocol

HTTP adalah protocol application layer dengan model request-response. Client mengirim request, server membalas response. Client bisa browser, curl, mobile app, API client, proxy, health checker, atau scanner internal. Server bisa web server, reverse proxy, API gateway, load balancer, object storage, atau aplikasi.

Alur sederhana:

```text
Client
-> TCP connection
-> optional TLS handshake for HTTPS
-> HTTP request
<- HTTP response
```

HTTP berada di atas transport:

| Layer | Contoh |
|---|---|
| Application | HTTP |
| Presentation/Security | TLS untuk HTTPS |
| Transport | TCP untuk HTTP/1.1 dan HTTP/2, QUIC/UDP untuk HTTP/3 |
| Network | IPv4/IPv6 |

HTTP plaintext biasanya memakai TCP port `80`. HTTPS biasanya memakai TCP port `443`. HTTP/3 memakai QUIC di atas UDP; untuk HTTPS default-nya umum di UDP `443`, tetapi endpoint HTTP/3 juga bisa diiklankan pada port UDP lain lewat mekanisme seperti Alt-Svc.

### 1.2 URL, Origin, Authority, dan Path

URL bukan hanya string. URL menentukan scheme, authority, path, query, dan fragment.

Contoh:

```text
https://www.example.com:443/app/search?q=linux#section-1
```

Bagian URL:

| Bagian | Contoh | Arti |
|---|---|---|
| scheme | `https` | protocol |
| host | `www.example.com` | nama host |
| port | `443` | port tujuan |
| path | `/app/search` | resource path |
| query | `q=linux` | parameter query |
| fragment | `section-1` | hanya dipakai client/browser, tidak dikirim ke server dalam HTTP request |

Origin adalah kombinasi:

```text
scheme + host + port
```

Contoh origin berbeda:

| URL | Origin |
|---|---|
| `https://example.com` | `https://example.com:443` |
| `http://example.com` | `http://example.com:80` |
| `https://example.com:8443` | `https://example.com:8443` |
| `https://api.example.com` | `https://api.example.com:443` |

Origin penting untuk browser security seperti same-origin policy, CORS, cookie scoping, dan storage isolation.

### 1.3 Request, Response, Header, dan Body

HTTP request terdiri dari:

```text
request line
headers
blank line
optional body
```

Contoh request:

```text
GET / HTTP/1.1
Host: example.com
User-Agent: curl/8.x
Accept: */*
Connection: close

```

HTTP response terdiri dari:

```text
status line
headers
blank line
optional body
```

Contoh response:

```text
HTTP/1.1 200 OK
Date: Sun, 24 May 2026 00:00:00 GMT
Content-Type: text/html
Content-Length: 13
Connection: close

Hello, world!
```

Header adalah metadata. Body adalah data utama. Untuk `GET`, body jarang dipakai. Untuk `POST`, `PUT`, dan `PATCH`, body umum dipakai.

Praktik langsung:

```bash
# Melihat request/response HTTP yang dibuat curl secara verbose.
curl -v http://example.com/

# Mengambil hanya response header agar fokus ke status line dan metadata.
curl -I http://example.com/
```

Observasi:

- request line yang dikirim client
- status line yang dikembalikan server
- header seperti `Content-Type`, `Content-Length`, `Date`, dan `Connection`
- apakah server memberi body atau hanya header

### 1.4 HTTP Methods

Method memberi tahu maksud request.

| Method | Fungsi Umum | Keterangan |
|---|---|---|
| GET | mengambil resource | sebaiknya safe dan idempotent |
| HEAD | seperti GET tapi tanpa body response | cek header/metadata |
| POST | mengirim data atau membuat proses baru | tidak harus idempotent |
| PUT | mengganti/membuat resource pada target | idempotent secara konsep |
| PATCH | mengubah sebagian resource | tidak selalu idempotent |
| DELETE | menghapus resource | idempotent secara konsep |
| OPTIONS | menanyakan capability server | sering dipakai CORS preflight |
| CONNECT | membuat tunnel lewat proxy | umum untuk HTTPS via proxy |
| TRACE | diagnostic echo | sering disabled karena risiko |

Safe berarti method tidak dimaksudkan mengubah state server. Idempotent berarti request yang sama diulang beberapa kali menghasilkan efek akhir yang sama.

### 1.5 Status Codes

Status code adalah ringkasan hasil response.

| Range | Arti | Contoh |
|---:|---|---|
| 1xx | informational | `100 Continue` |
| 2xx | success | `200 OK`, `201 Created`, `204 No Content` |
| 3xx | redirection | `301 Moved Permanently`, `302 Found`, `304 Not Modified` |
| 4xx | client error | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` |
| 5xx | server error | `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout` |

Perbedaan yang sering penting:

| Status | Arti |
|---:|---|
| 401 | belum/ gagal authentication, biasanya ada `WWW-Authenticate` |
| 403 | authenticated atau tidak, tetapi tidak boleh akses |
| 404 | resource tidak ditemukan atau sengaja disembunyikan |
| 405 | method tidak diizinkan |
| 409 | conflict state |
| 429 | rate limited |
| 502 | gateway/proxy menerima response buruk dari upstream |
| 503 | service unavailable |
| 504 | gateway/proxy timeout ke upstream |

---

## 2.0 HTTP/1.1 Low-Level

**Fokus teknis:** struktur HTTP/1.1 manual terlihat dari CRLF, Host, dan blank line.

```bash
# Membuka koneksi TCP plaintext ke web server Google pada port 80.
telnet www.google.com 80

# Mengirim raw HTTP request dengan CRLF lewat netcat.
printf 'GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n' | nc example.com 80
```

Aspek teknis: request line, `Host`, blank line, response status, dan perbedaan request manual dengan browser.

### 2.1 Request Line dan CRLF

HTTP/1.1 memakai format teks. Line ending HTTP adalah `CRLF`, yaitu `\r\n`. Header selesai saat ada blank line.

Request line:

```text
METHOD SP request-target SP HTTP-version CRLF
```

Contoh:

```text
GET /index.html HTTP/1.1
```

Header:

```text
Header-Name: value
```

Blank line:

```text
\r\n
```

Request lengkap secara byte-level konseptual:

```text
GET / HTTP/1.1\r\n
Host: example.com\r\n
Connection: close\r\n
\r\n
```

Kalau blank line tidak dikirim, server bisa menunggu karena belum tahu header selesai.

Praktik langsung dengan server publik:

```bash
# Membuka koneksi TCP plaintext ke web server Google pada port 80.
telnet www.google.com 80
```

Setelah koneksi terbuka, ketik manual:

```text
GET / HTTP/1.1
Host: www.google.com
Connection: close

```

Request HTTP/1.1 diakhiri dengan blank line setelah `Connection: close`. Response publik bisa berubah, tetapi format dasarnya tetap berupa request line, header, blank line, lalu response.

### 2.2 Host Header dan Virtual Hosting

HTTP/1.1 membutuhkan `Host` header. Satu IP bisa melayani banyak domain. Web server memakai `Host` untuk memilih virtual host.

Contoh:

```text
GET / HTTP/1.1
Host: app.example.com

```

Tanpa `Host`, server modern bisa membalas `400 Bad Request` atau memilih default virtual host.

Host-related behavior:

| Item | Fungsi |
|---|---|
| DNS | mengubah hostname menjadi IP |
| TCP | koneksi ke IP dan port |
| TLS SNI | memilih certificate saat HTTPS |
| HTTP Host header | memilih virtual host/app routing |

Untuk HTTPS, hostname bisa muncul di dua tempat:

| Tempat | Layer | Fungsi |
|---|---|---|
| SNI | TLS handshake | pilih certificate |
| Host header | HTTP | pilih virtual host/routing |

Jika SNI dan Host berbeda, behavior bergantung pada server/proxy. Ini penting untuk troubleshooting dan web security testing di lab.

Praktik langsung:

```bash
# Memaksa hostname resolve ke IP tertentu tanpa mengubah DNS sistem.
curl --resolve example.com:443:<server-ip> https://example.com/

# Mengarahkan koneksi ke endpoint lain sambil mempertahankan URL, Host, dan SNI.
curl --connect-to example.com:443:127.0.0.1:8443 https://example.com/
```

Konsep penting:

- `--resolve` memengaruhi DNS resolution di sisi curl
- `--connect-to` memengaruhi tujuan koneksi TCP/TLS
- URL tetap menentukan Host/SNI yang dianggap client sebagai tujuan logis
- teknik ini berguna untuk menguji routing, reverse proxy, staging origin, dan certificate behavior

### 2.3 Content-Length, Transfer-Encoding, dan Chunked Body

HTTP perlu tahu batas body. Dua mekanisme umum:

| Mekanisme | Arti |
|---|---|
| `Content-Length` | panjang body dalam byte |
| `Transfer-Encoding: chunked` | body dikirim dalam chunk |

POST dengan `Content-Length`:

```text
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 28
Connection: close

username=alice&password=test
```

Chunked body:

```text
POST /upload HTTP/1.1
Host: example.com
Transfer-Encoding: chunked
Connection: close

5
hello
6
 world
0

```

Chunk format:

```text
chunk-size-in-hex CRLF
chunk-data CRLF
...
0 CRLF
optional trailers CRLF
CRLF
```

Hal penting:

- `Content-Length` harus cocok dengan jumlah byte body.
- Jika terlalu pendek, sisa body bisa dianggap request berikutnya pada koneksi yang sama.
- Jika terlalu panjang, server bisa menunggu data tambahan.
- Perbedaan parsing antara proxy dan backend bisa menjadi sumber bug serius.

Praktik langsung untuk melihat body mentah:

Terminal 1:

```bash
# Membuka listener lokal untuk menerima request mentah.
nc -l 8080
```

Terminal 2:

```bash
# Mengirim POST dengan Content-Length yang cocok dengan panjang body.
printf 'POST /login HTTP/1.1\r\nHost: localhost\r\nContent-Type: application/x-www-form-urlencoded\r\nContent-Length: 28\r\nConnection: close\r\n\r\nusername=alice&password=test' | nc 127.0.0.1 8080
```

Amati di Terminal 1:

- header berhenti setelah blank line
- body mulai setelah blank line
- `Content-Length` menghitung byte body, bukan jumlah karakter header

Praktik chunked:

Terminal 1:

```bash
# Membuka listener lokal untuk melihat chunked body mentah.
nc -l 8080
```

Terminal 2:

```bash
# Mengirim body dengan Transfer-Encoding chunked.
printf 'POST /upload HTTP/1.1\r\nHost: localhost\r\nTransfer-Encoding: chunked\r\nConnection: close\r\n\r\n5\r\nhello\r\n6\r\n world\r\n0\r\n\r\n' | nc 127.0.0.1 8080
```

Observasi:

- angka `5` dan `6` adalah ukuran chunk dalam hexadecimal
- `0` berarti body selesai
- chunked membuat batas body ditentukan oleh framing chunk, bukan `Content-Length`

### 2.4 Keep-Alive, Connection Close, dan Pipelining

HTTP/1.1 default-nya persistent connection kecuali ada `Connection: close`. Artinya satu koneksi TCP bisa dipakai untuk lebih dari satu request.

Contoh:

```text
GET /one HTTP/1.1
Host: example.com

GET /two HTTP/1.1
Host: example.com

```

Pipelining HTTP/1.1 jarang dipakai di browser modern karena banyak masalah kompatibilitas/proxy. Tetapi konsep persistent connection tetap penting karena parsing body dan batas request menjadi krusial.

Header umum:

| Header | Arti |
|---|---|
| `Connection: close` | tutup koneksi setelah response |
| `Connection: keep-alive` | minta koneksi tetap terbuka, lebih relevan di HTTP/1.0 |
| `Keep-Alive` | parameter timeout/max pada beberapa server |

### 2.5 Raw HTTP dengan Telnet, Netcat, dan OpenSSL

`telnet` dan `nc` berguna untuk melihat HTTP plaintext. Untuk HTTPS, gunakan `openssl s_client` karena telnet tidak melakukan TLS.

Raw HTTP via telnet:

```bash
# Membuka koneksi TCP plaintext ke port HTTP.
telnet example.com 80
```

Request manual:

```text
GET / HTTP/1.1
Host: example.com
Connection: close

```

Raw HTTP via netcat:

```bash
# Mengirim raw HTTP request dengan CRLF ke port 80.
printf 'GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n' | nc example.com 80
```

Raw HTTPS via OpenSSL:

```bash
# Membuka koneksi TLS ke HTTPS dengan SNI.
openssl s_client -connect example.com:443 -servername example.com -quiet
```

Ketik request:

```text
GET / HTTP/1.1
Host: example.com
Connection: close

```

Raw HTTPS satu baris:

```bash
# Mengirim raw HTTP request melalui koneksi TLS.
printf 'GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n' | openssl s_client -connect example.com:443 -servername example.com -quiet
```

Contoh HTTPS publik:

```bash
# Membuka TLS ke Google dengan SNI untuk HTTP request manual.
openssl s_client -connect www.google.com:443 -servername www.google.com -quiet
```

Request manual:

```text
GET / HTTP/1.1
Host: www.google.com
Connection: close

```

Perhatikan perbedaannya dengan telnet: sebelum HTTP request terlihat oleh server, TLS handshake harus selesai dulu. Karena itu `telnet` cocok untuk HTTP plaintext, sedangkan HTTPS perlu tool yang bisa TLS seperti `openssl s_client`.

### 2.6 Request Target Forms

Request target adalah bagian setelah method pada request line. Bentuknya berubah tergantung apakah client bicara langsung ke origin server, proxy, atau tunnel.

Format umum:

```text
METHOD request-target HTTP-version
```

Jenis request target:

| Form | Contoh | Dipakai Saat |
|---|---|---|
| origin-form | `GET /path?q=1 HTTP/1.1` | request biasa ke origin server |
| absolute-form | `GET http://example.com/path HTTP/1.1` | request ke forward proxy |
| authority-form | `CONNECT example.com:443 HTTP/1.1` | membuat tunnel proxy |
| asterisk-form | `OPTIONS * HTTP/1.1` | capability server secara umum |

Origin-form:

```text
GET /admin?q=1 HTTP/1.1
Host: example.com

```

Absolute-form:

```text
GET http://example.com/admin?q=1 HTTP/1.1
Host: example.com

```

CONNECT authority-form:

```text
CONNECT example.com:443 HTTP/1.1
Host: example.com:443

```

Hal yang penting untuk web security:

- proxy dan backend bisa memperlakukan absolute URL berbeda dari path biasa
- `Host` header dan authority di request target bisa tidak sama
- proxy chain bisa menormalisasi path sebelum backend melihat request
- routing bisa dipengaruhi oleh `Host`, SNI, absolute URL, dan konfigurasi reverse proxy

### 2.7 Hop-by-Hop dan End-to-End Headers

Header tidak semuanya punya scope yang sama. Ada header yang hanya berlaku untuk satu koneksi antar dua hop, dan ada header yang seharusnya diteruskan sampai origin/backend.

Jenis header:

| Jenis | Arti | Contoh |
|---|---|---|
| hop-by-hop | hanya untuk koneksi saat ini | `Connection`, `Keep-Alive`, `TE`, `Trailer`, `Upgrade` |
| end-to-end | diteruskan ke tujuan akhir | `Host`, `Authorization`, `Cookie`, `Content-Type`, `Cache-Control` |

`Connection` bisa menyebut header lain yang harus diperlakukan sebagai hop-by-hop.

Contoh:

```text
Connection: close, X-Debug
X-Debug: true
```

Dalam desain proxy yang benar, header yang disebut oleh `Connection` tidak boleh diteruskan ke backend sebagai end-to-end header. Jika proxy salah menangani ini, behavior backend bisa berbeda dari yang diharapkan.

Header `Upgrade`:

| Header | Fungsi |
|---|---|
| `Connection: Upgrade` | menyatakan koneksi ingin upgrade protocol |
| `Upgrade: websocket` | contoh upgrade ke WebSocket |

- hop-by-hop header harus dibersihkan di proxy boundary
- forwarding header dari internet harus di-overwrite oleh proxy tepercaya
- perbedaan handling header antar proxy/backend sering menjadi akar bug parsing

---

## 3.0 Headers, Cookies, and Auth

**Fokus teknis:** header, cookie, dan auth sebagai input yang bisa dikontrol dan diamati.

```bash
# Mengirim header custom.
curl -v -H 'X-Test: hello' https://example.com/

# Menyimpan cookie dari response.
curl -v -c cookies.txt https://example.com/

# Mengirim cookie dari file.
curl -v -b cookies.txt https://example.com/
```

Aspek teknis: header yang dikirim, header yang dibalas, cookie scope, cookie attribute, dan apakah response berubah karena input header/cookie.

### 3.1 Header Penting

Header memengaruhi routing, auth, cache, format body, session, proxy, dan security.

| Header | Fungsi |
|---|---|
| `Host` | virtual host/routing |
| `User-Agent` | identitas client |
| `Accept` | format response yang diterima |
| `Content-Type` | format body request/response |
| `Content-Length` | panjang body |
| `Transfer-Encoding` | cara body dikirim |
| `Connection` | kontrol koneksi |
| `Cookie` | cookie dari client |
| `Set-Cookie` | cookie dari server |
| `Authorization` | credential/token |
| `Location` | target redirect |
| `Origin` | origin request browser tertentu |
| `Referer` | halaman asal request |
| `Forwarded` | info proxy standar |
| `X-Forwarded-For` | client IP menurut proxy |
| `X-Forwarded-Proto` | scheme asli menurut proxy |
| `X-Forwarded-Host` | host asli menurut proxy |

Header name case-insensitive. `Host`, `host`, dan `HOST` secara konsep nama header yang sama. Namun aplikasi/proxy yang buruk bisa punya parsing berbeda.

### 3.2 Cookies and Session

Cookie adalah state kecil yang disimpan client dan dikirim kembali ke server sesuai scope.

Server mengirim cookie:

```text
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Lax; Path=/; Domain=example.com
```

Client mengirim cookie:

```text
Cookie: session_id=abc123
```

Attribute penting:

| Attribute | Fungsi |
|---|---|
| `HttpOnly` | JavaScript tidak bisa membaca cookie |
| `Secure` | cookie hanya dikirim lewat HTTPS |
| `SameSite` | membatasi pengiriman cross-site |
| `Path` | scope path |
| `Domain` | scope domain |
| `Expires` / `Max-Age` | umur cookie |

Keterangan:

- Cookie adalah bearer token jika berisi session identifier.
- Jika cookie dicuri, attacker bisa memakai session sampai expired/revoked.
- `HttpOnly` membantu melawan pencurian cookie via XSS, tetapi tidak mencegah request berjalan dari browser victim.
- `SameSite` membantu mengurangi CSRF, tetapi bukan pengganti desain CSRF token dalam semua scenario.

### 3.3 Authorization Header

`Authorization` dipakai untuk mengirim credential atau token.

Contoh Basic:

```text
Authorization: Basic base64(username:password)
```

Contoh Bearer:

```text
Authorization: Bearer eyJ...
```

Perbedaan:

| Scheme | Keterangan |
|---|---|
| Basic | harus lewat HTTPS, mudah direplay jika bocor |
| Bearer | siapa pun yang memegang token bisa memakai |
| Digest | legacy/lebih jarang |
| mTLS | identity via certificate client |

Jangan menaruh token di URL query jika bisa dihindari karena URL sering masuk log, history, referer, dan monitoring.

### 3.4 CORS, Origin, and Browser Enforcement

CORS adalah mekanisme browser untuk mengontrol apakah JavaScript dari satu origin boleh membaca response dari origin lain. CORS bukan firewall server-to-server. `curl` dan backend service tidak dibatasi CORS seperti browser.

Header CORS:

| Header | Fungsi |
|---|---|
| `Origin` | origin halaman pemanggil |
| `Access-Control-Allow-Origin` | origin yang boleh membaca response |
| `Access-Control-Allow-Credentials` | apakah credential boleh ikut |
| `Access-Control-Allow-Methods` | method yang boleh |
| `Access-Control-Allow-Headers` | header yang boleh |

Preflight:

```text
OPTIONS /api HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type, authorization

```

Risiko umum:

| Misconfiguration | Risiko |
|---|---|
| allow origin terlalu luas | data bisa dibaca origin tidak dipercaya |
| credentials + reflected origin | session user bisa dipakai lintas origin |
| mengira CORS melindungi API dari curl | salah model ancaman |

### 3.5 Content Negotiation and Compression

Content negotiation adalah cara client memberi tahu format yang diinginkan dan server memilih response yang cocok.

Header umum:

| Header | Arah | Fungsi |
|---|---|---|
| `Accept` | request | tipe media yang diterima client |
| `Accept-Language` | request | bahasa yang diinginkan client |
| `Accept-Encoding` | request | compression yang diterima client |
| `Content-Type` | request/response | format body |
| `Content-Encoding` | response | compression yang dipakai pada body |
| `Vary` | response | header yang memengaruhi variasi cache |

Contoh request:

```text
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
Accept-Encoding: gzip, br

```

Contoh response compressed:

```text
HTTP/1.1 200 OK
Content-Type: application/json
Content-Encoding: gzip
Vary: Accept-Encoding

```

Perbedaan penting:

| Header | Arti |
|---|---|
| `Content-Type` | bentuk data sebelum/ketika dipahami aplikasi |
| `Content-Encoding` | transformasi encoding seperti gzip/br |
| `Transfer-Encoding` | cara body dikirim di HTTP/1.1 |

- jika response ter-compress, capture payload tidak langsung terbaca sebagai text
- cache harus memperhatikan `Vary`, terutama untuk `Accept-Encoding` dan `Origin`
- mismatch `Content-Type` bisa menyebabkan browser menebak tipe jika `nosniff` tidak dipakai
- API sebaiknya memvalidasi `Content-Type`, bukan hanya melihat body terlihat seperti JSON

### 3.6 Multipart Form Upload

`multipart/form-data` sering dipakai untuk upload file. Body dipisah oleh boundary.

Contoh request konseptual:

```text
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----abc
Content-Length: <panjang body dalam byte>

------abc
Content-Disposition: form-data; name="description"

sample file
------abc
Content-Disposition: form-data; name="file"; filename="note.txt"
Content-Type: text/plain

hello
------abc--
```

Aspek teknis:

| Area | Risiko |
|---|---|
| filename | path traversal, karakter aneh, double extension |
| content type | bisa dipalsukan client |
| file size | resource exhaustion |
| storage path | file executable di web root |
| parsing library | boundary atau encoding edge case |

Validasi upload sebaiknya memakai kombinasi allowlist extension, magic bytes bila perlu, size limit, storage terpisah dari web root, nama file random, dan permission minimal.

---

## 4.0 Caching, Proxy, and TLS

**Fokus teknis:** uji cache, proxy header, TLS certificate, dan SNI sebagai satu jalur request.

```bash
# Melihat cache-related dan security header.
curl -I https://example.com/

# Melihat certificate dan handshake TLS dengan SNI.
openssl s_client -connect example.com:443 -servername example.com

# Memaksa hostname resolve ke IP tertentu tanpa mengubah DNS sistem.
curl --resolve example.com:443:<server-ip> https://example.com/
```

Aspek teknis: certificate subject/SAN, issuer, cache header, `Vary`, proxy header, dan apakah hostname/SNI/Host konsisten.

### 4.1 Cache-Control, ETag, and Conditional Request

HTTP cache bisa ada di browser, proxy, CDN, reverse proxy, atau application layer.

Header cache:

| Header | Fungsi |
|---|---|
| `Cache-Control` | aturan cache utama |
| `ETag` | validator object |
| `Last-Modified` | waktu modifikasi |
| `If-None-Match` | conditional request berdasarkan ETag |
| `If-Modified-Since` | conditional request berdasarkan waktu |
| `Vary` | response berbeda berdasarkan header tertentu |

Contoh:

```text
Cache-Control: private, no-store
```

Arti umum:

| Directive | Arti |
|---|---|
| `no-store` | jangan simpan response |
| `no-cache` | boleh simpan, tetapi harus revalidate |
| `private` | hanya cache private/browser |
| `public` | boleh shared cache |
| `max-age` | umur cache dalam detik |
| `s-maxage` | umur untuk shared cache |

Untuk data sensitif, `no-store` biasanya lebih aman.

### 4.2 Forward Proxy, Reverse Proxy, and Load Balancer

Proxy mengubah path request. Ini penting karena bug sering muncul saat proxy dan backend tidak sepakat membaca request.

| Komponen | Posisi | Fungsi |
|---|---|---|
| Forward proxy | dekat client | akses internet via proxy |
| Reverse proxy | depan server | menerima request publik dan forward ke backend |
| Load balancer | depan backend pool | membagi traffic |
| CDN | edge global | cache dan acceleration |
| WAF | depan aplikasi | inspeksi HTTP/security policy |

Header proxy:

| Header | Fungsi |
|---|---|
| `X-Forwarded-For` | IP client asli menurut proxy |
| `X-Forwarded-Proto` | scheme asli `http`/`https` |
| `X-Forwarded-Host` | host asli |
| `Forwarded` | standar untuk info proxy |

Jangan percaya header `X-Forwarded-*` langsung dari internet kecuali header itu diset/di-overwrite oleh proxy tepercaya.

### 4.3 HTTPS, TLS, SNI, and Certificate

HTTPS adalah HTTP di atas TLS. TLS memberi confidentiality, integrity, dan server authentication.

Alur:

```text
DNS lookup
-> TCP handshake ke 443
-> TLS ClientHello dengan SNI
-> Server memilih certificate
-> TLS key exchange selesai
-> HTTP request dikirim dalam TLS
```

SNI atau Server Name Indication memungkinkan server memilih certificate berdasarkan hostname sebelum HTTP request terlihat.

Yang dicek saat HTTPS gagal:

| Gejala | Kemungkinan |
|---|---|
| certificate expired | `NotAfter` lewat |
| hostname mismatch | SAN tidak berisi hostname |
| unknown CA | chain tidak trusted |
| handshake failure | TLS version/cipher mismatch |
| works by IP but not hostname | SNI/Host/certificate issue |

Praktik langsung:

```bash
# Melihat certificate dan handshake TLS dengan SNI.
openssl s_client -connect example.com:443 -servername example.com

# Melihat certificate chain dari server.
openssl s_client -connect example.com:443 -servername example.com -showcerts

# Melihat tanggal berlaku certificate.
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates

# Melihat subject dan issuer certificate.
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer
```

Observasi:

- apakah certificate cocok dengan hostname
- apakah chain certificate lengkap
- kapan certificate expired
- apakah handshake gagal sebelum HTTP request dikirim

---

## 5.0 HTTP Versions

**Fokus teknis:** bandingkan HTTP/1.1, HTTP/2, HTTP/3, dan ALPN negotiation.

```bash
# Memaksa HTTP/1.1.
curl --http1.1 -v https://example.com/

# Memakai HTTP/2 jika server mendukung.
curl --http2 -v https://example.com/

# Melihat ALPN negotiation untuk HTTP/2 atau HTTP/1.1.
openssl s_client -connect example.com:443 -servername example.com -alpn h2,http/1.1
```

Aspek teknis: protocol yang dipilih, ALPN result, perbedaan header, dan apakah error terjadi di DNS, TCP, TLS, ALPN, atau HTTP.

### 5.1 HTTP/1.1

HTTP/1.1 berbasis teks dan biasanya satu request-response per connection flow, walaupun connection bisa persistent. Karena formatnya teks, HTTP/1.1 mudah dipelajari dengan telnet/netcat.

Karakter:

- text-based
- header dan body dipisah blank line
- `Host` header penting
- persistent connection default
- body length penting untuk parsing

### 5.2 HTTP/2

HTTP/2 memakai framing binary, multiplexing, stream, header compression, dan biasanya berjalan di atas TLS untuk browser.

Karakter:

| Fitur | Arti |
|---|---|
| binary framing | bukan raw text seperti HTTP/1.1 |
| stream | banyak stream dalam satu koneksi |
| multiplexing | request paralel tanpa banyak TCP connection |
| HPACK | header compression |

HTTP/2 tidak nyaman dites dengan telnet karena bukan format teks sederhana.

### 5.3 HTTP/3

HTTP/3 memakai QUIC di atas UDP. Untuk HTTPS, port yang paling umum adalah `443/UDP`, tetapi server dapat mengiklankan endpoint HTTP/3 pada port UDP lain. QUIC menggabungkan transport security dan transport behavior modern.

Karakter:

| Fitur | Arti |
|---|---|
| QUIC | transport di atas UDP |
| TLS 1.3 integrated | security bagian dari QUIC |
| stream multiplexing | mengurangi head-of-line blocking TCP |
| connection migration | koneksi bisa bertahan saat IP berubah dalam scenario tertentu |

### 5.4 ALPN and Version Negotiation

ALPN atau Application-Layer Protocol Negotiation adalah bagian dari TLS handshake yang memungkinkan client dan server menyepakati protocol aplikasi, misalnya `http/1.1` atau `h2`.

Contoh alur HTTPS modern:

```text
ClientHello
  SNI: example.com
  ALPN: h2, http/1.1

ServerHello
  ALPN selected: h2
```

Kenapa penting:

| Area | Dampak |
|---|---|
| troubleshooting | server bisa memilih HTTP/1.1 atau HTTP/2 berbeda |
| proxy/CDN | protocol client-to-edge bisa berbeda dari edge-to-origin |
| testing | hasil `curl` bisa berubah karena negotiation |
| security | HTTP/2 dan HTTP/1.1 punya framing/parsing yang berbeda |

HTTP/2 memiliki pseudo-header yang menggantikan request line HTTP/1.1.

Mapping konsep:

| HTTP/1.1 | HTTP/2 pseudo-header |
|---|---|
| method di request line | `:method` |
| path/query di request line | `:path` |
| scheme | `:scheme` |
| Host/authority | `:authority` |

Contoh konsep HTTP/2:

```text
:method: GET
:scheme: https
:authority: example.com
:path: /
user-agent: curl/8.x
```

Keterangan: HTTP/2 bukan sekadar HTTP/1.1 yang dikompresi. Ia memakai binary framing, stream, dan aturan header berbeda. Saat ada proxy yang menerjemahkan HTTP/2 ke HTTP/1.1, pastikan mapping `:authority`, `Host`, path, dan header lain konsisten.

Praktik langsung:

```bash
# Memaksa HTTP/1.1 agar bisa dibandingkan dengan HTTP/2.
curl --http1.1 -v https://example.com/

# Memakai HTTP/2 jika curl dan server mendukung.
curl --http2 -v https://example.com/

# Mencoba HTTP/3 jika curl dan server mendukung.
curl --http3 -v https://example.com/

# Melihat ALPN negotiation untuk HTTP/2 atau HTTP/1.1.
openssl s_client -connect example.com:443 -servername example.com -alpn h2,http/1.1
```

Perbandingan:

- protocol yang benar-benar dipilih
- apakah response header sama atau berbeda
- apakah proxy/CDN memilih protocol berbeda di edge dan origin
- apakah error muncul di TLS, ALPN, transport, atau HTTP layer

---

## 6.0 Web Security Notes

**Fokus teknis:** baca web security dari parsing, trust boundary, cache, header, dan URL handling.

```bash
# Tidak menormalisasi path tertentu saat testing di environment berizin.
curl --path-as-is -v 'http://localhost/a/../b'

# Mengirim Host header custom ke endpoint lokal.
curl -v -H 'Host: example.com' http://127.0.0.1/

# Mengirim header custom untuk melihat apakah response/cache berubah.
curl -I -H 'X-Test: cache-probe' https://example.com/
```

Aspek teknis: trust boundary, parser yang membaca request, header yang dipercaya, cache key, dan validasi defensive yang harus diterapkan.

### 6.1 Request Parsing and Trust Boundaries

HTTP sering melewati banyak komponen:

```text
Browser
-> CDN
-> WAF
-> Load balancer
-> Reverse proxy
-> App server
-> Framework/router
```

Setiap komponen membaca request. Risiko muncul jika komponen membaca request dengan aturan berbeda.

Trust boundary:

| Boundary | Risiko |
|---|---|
| client ke CDN/WAF | input attacker |
| proxy ke backend | header bisa ditambah/dihapus |
| backend ke app | routing/path normalization |
| app ke internal service | SSRF dan trust internal |

### 6.2 Header-Based Security Controls

Security headers membantu browser menerapkan policy.

| Header | Fungsi |
|---|---|
| `Strict-Transport-Security` | memaksa HTTPS untuk domain |
| `Content-Security-Policy` | membatasi sumber script/content |
| `X-Content-Type-Options` | mencegah MIME sniffing |
| `Referrer-Policy` | mengontrol referrer |
| `Permissions-Policy` | membatasi browser features |
| `Set-Cookie` attributes | melindungi cookie |

Contoh:

```text
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
Referrer-Policy: no-referrer
```

Praktik langsung:

```bash
# Mengambil security header dari response HTTPS.
curl -I https://example.com/

# Melihat header lengkap dan proses redirect jika ada.
curl -L -I -v https://example.com/
```

Observasi:

- apakah `Strict-Transport-Security` hanya muncul di HTTPS
- apakah ada `Content-Security-Policy`
- apakah cookie memakai `Secure`, `HttpOnly`, dan `SameSite`
- apakah redirect dari HTTP ke HTTPS terjadi sebelum header security utama muncul

### 6.3 Request Smuggling Concepts

Request smuggling adalah kelas bug parsing HTTP saat front-end dan back-end tidak sepakat menentukan batas request. Ini biasanya berkaitan dengan `Content-Length`, `Transfer-Encoding`, chunked encoding, atau normalisasi header.

Konsep defensif:

| Pola | Arti |
|---|---|
| CL.TE | front-end percaya Content-Length, back-end percaya Transfer-Encoding |
| TE.CL | front-end percaya Transfer-Encoding, back-end percaya Content-Length |
| duplicate header | dua header serupa diparse beda |
| obs-fold/whitespace | whitespace tidak standar diparse beda |

Poin teknis:

- bagaimana request body dibatasi
- bagaimana proxy dan backend membaca header
- apakah koneksi persistent membuat request berikutnya terdampak
- apakah server menormalisasi header secara konsisten

Keterangan: uji request smuggling hanya di lab atau target yang berada dalam scope izin. Payload yang salah bisa mengganggu user lain karena memengaruhi koneksi persistent.

### 6.4 SSRF, Host Header, and URL Confusion

SSRF terjadi saat server membuat request ke URL yang dikendalikan user. Host header attack terjadi saat aplikasi mempercayai `Host` untuk membuat link, reset password URL, routing, atau security decision.

URL confusion bisa muncul dari:

| Area | Contoh |
|---|---|
| encoded path | `%2f`, `%2e` |
| mixed case host | `EXAMPLE.com` |
| userinfo | `https://user@host/path` |
| trailing dot | `example.com.` |
| IPv6 literal | `[::1]` |
| alternate IP notation | decimal/octal/hex pada parser tertentu |
| redirect chain | allowlist hanya cek URL awal |

Defensive principle:

- parse URL dengan library standar
- allowlist destination berdasarkan canonical host/IP
- resolve DNS dan cek IP range hasil resolve
- block metadata/internal IP jika tidak perlu
- jangan percaya `Host` dari client tanpa validasi
- overwrite forwarding headers di reverse proxy

### 6.5 Cache Poisoning and Web Cache Deception

Cache bug muncul saat cache key tidak sama dengan faktor yang membuat response berubah. Ini penting karena CDN/proxy bisa menyimpan response dan memberikannya ke user lain.

Istilah:

| Istilah | Arti |
|---|---|
| cache key | bagian request yang dipakai cache untuk membedakan object |
| cache hit | response diambil dari cache |
| cache miss | cache harus mengambil dari origin |
| unkeyed input | input memengaruhi response tetapi tidak masuk cache key |
| shared cache | cache dipakai banyak user, misalnya CDN/proxy |

Cache poisoning secara konsep:

```text
Request dengan header/query tertentu
-> origin membuat response berbeda
-> cache menyimpan response
-> user lain menerima response yang sudah terpengaruh
```

Web cache deception secara konsep:

```text
User membuka URL yang terlihat static
-> aplikasi mengembalikan data private
-> cache mengira response boleh disimpan
-> data private berisiko tersaji dari cache
```

Header yang sering relevan:

| Header | Peran |
|---|---|
| `Cache-Control` | menentukan apakah response boleh disimpan |
| `Vary` | menyatakan request header yang memengaruhi response |
| `Set-Cookie` | tanda response bisa bersifat user-specific |
| `Authorization` | request authenticated biasanya tidak boleh masuk shared cache sembarangan |
| `X-Cache` | indikator hit/miss pada beberapa CDN/proxy |

Checklist defensif:

- response private memakai `Cache-Control: private, no-store` bila perlu
- response authenticated tidak disimpan shared cache kecuali desainnya jelas
- `Vary` mencakup header yang benar-benar memengaruhi response
- static dan dynamic route tidak ambigu
- CDN/proxy rule didokumentasikan dan diuji dengan user berbeda

Praktik langsung:

```bash
# Melihat cache-related header dari response.
curl -I https://example.com/

# Meminta response compressed agar terlihat apakah Vary memperhitungkan Accept-Encoding.
curl --compressed -I https://example.com/

# Mengirim header custom untuk melihat apakah response berubah.
curl -I -H 'X-Test: cache-probe' https://example.com/
```

Observasi:

- apakah ada `Cache-Control`, `ETag`, `Age`, `Vary`, atau `X-Cache`
- apakah response berubah ketika header request berubah
- apakah response yang mengandung data user punya instruksi cache yang aman
- apakah cache behavior berasal dari origin, reverse proxy, atau CDN
