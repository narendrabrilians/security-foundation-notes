# Security Foundation Notes

Catatan pribadi untuk membangun fondasi security, networking, Linux, Windows, Windows Server, Active Directory, HTTP, dan security operations.

Repo ini dibuat sebagai catatan belajar. Fokusnya bukan hanya definisi, tetapi pemahaman konsep, command yang bisa dipakai di lab, troubleshooting berbasis bukti, dan cara berpikir yang rapi untuk membangun fondasi security.

## Notes

| File | Fokus |
|---|---|
| [01 - Network.md](./01-Network.md) | OSI/TCP-IP, switching, routing, IPv4/IPv6, wireless, network security, troubleshooting |
| [02 - Linux.md](./02-Linux.md) | Linux administration, filesystem, process, service, networking, security, logging, automation |
| [03 - Windows.md](./03-Windows.md) | Windows client, local administration, registry, services, PowerShell, security, troubleshooting |
| [04 - Windows Server.md](./04-Windows-Server.md) | Server roles, DNS, DHCP, SMB, IIS, Hyper-V, clustering, hardening, monitoring |
| [05 - Active Directory.md](./05-Active-Directory.md) | Domain, forest, DC, DNS AD, Kerberos, LDAP, GPO, replication, FSMO, AD security |
| [06 - Security.md](./06-Security.md) | Security concepts, threats, architecture, operations, IAM, monitoring, IR, risk, playbooks |
| [07 - HTTP.md](./07-HTTP.md) | HTTP fundamentals, raw request/response, headers, cookies, TLS, proxy, cache, low-level testing |

## Suggested Learning Path

1. [Network.md](./01-Network.md)
2. [Linux.md](./02-Linux.md)
3. [Windows.md](./03-Windows.md)
4. [Windows-Server.md](./04-Windows-Server.md)
5. [Active-Directory.md](./05-Active-Directory.md)
6. [Security.md](./06-Security.md)
7. [HTTP.md](./07-HTTP.md)

Urutan ini dipilih karena security yang kuat butuh fondasi infrastruktur. Network membantu memahami traffic dan failure domain. Linux dan Windows membantu memahami endpoint/server. Windows Server dan Active Directory membantu memahami identity dan enterprise environment. Security menyatukan semuanya menjadi risk, control, detection, response, dan governance. HTTP ditambahkan sebagai deep dive khusus karena web security butuh pemahaman protocol sampai level raw request, header, TLS, proxy, cache, dan command low-level.

## How to Use

- Baca konsep sebelum menjalankan command.
- Setelah masuk ke sebuah bab, jalankan bagian praktik di awal bab tersebut sebelum lanjut terlalu jauh.
- Praktikkan command di lab, bukan langsung di production.
- Gunakan snapshot sebelum eksperimen besar.
- Catat gejala, bukti, command, output, dan perubahan yang dilakukan.
- Saat troubleshooting, mulai dari scope, waktu kejadian, dependency, log, dan perubahan terakhir.
- Saat hardening, pastikan control tidak memutus kebutuhan operasional.

## Thanks

Terima kasih sudah membaca. Semoga catatan ini membantu proses belajar dan membangun fondasi security dengan lebih rapi.
