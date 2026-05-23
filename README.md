# Security Foundation Notes

Catatan pribadi untuk membangun fondasi security, system administration, networking, Windows, Linux, Windows Server, Active Directory, dan security operations.

Repo ini dibuat sebagai handbook belajar. Fokusnya bukan hanya definisi, tetapi pemahaman konsep, command yang bisa dipakai di lab, troubleshooting berbasis bukti, dan cara berpikir yang rapi untuk membangun fondasi security.

## Handbook

| File | Fokus |
|---|---|
| [Linux.md](./Linux.md) | Linux administration, filesystem, process, service, networking, security, logging, automation |
| [Network.md](./Network.md) | OSI/TCP-IP, switching, routing, IPv4/IPv6, wireless, network security, troubleshooting |
| [Windows.md](./Windows.md) | Windows client, local administration, registry, services, PowerShell, security, troubleshooting |
| [Windows-Server.md](./Windows-Server.md) | Server roles, DNS, DHCP, SMB, IIS, Hyper-V, clustering, hardening, monitoring |
| [Active-Directory.md](./Active-Directory.md) | Domain, forest, DC, DNS AD, Kerberos, LDAP, GPO, replication, FSMO, AD security |
| [Security.md](./Security.md) | Security concepts, threats, architecture, operations, IAM, monitoring, IR, risk, playbooks |

## Suggested Learning Path

1. [Network.md](./Network.md)
2. [Linux.md](./Linux.md)
3. [Windows.md](./Windows.md)
4. [Windows-Server.md](./Windows-Server.md)
5. [Active-Directory.md](./Active-Directory.md)
6. [Security.md](./Security.md)

Urutan ini dipilih karena security yang kuat butuh fondasi infrastruktur. Network membantu memahami traffic dan failure domain. Linux dan Windows membantu memahami endpoint/server. Windows Server dan Active Directory membantu memahami identity dan enterprise environment. Security menyatukan semuanya menjadi risk, control, detection, response, dan governance.

## How to Use

- Baca konsep sebelum menjalankan command.
- Praktikkan command di lab, bukan langsung di production.
- Gunakan snapshot sebelum eksperimen besar.
- Catat gejala, bukti, command, output, dan perubahan yang dilakukan.
- Saat troubleshooting, mulai dari scope, waktu kejadian, dependency, log, dan perubahan terakhir.
- Saat hardening, pastikan control tidak memutus kebutuhan operasional.

## Notes

Catatan ini akan terus berkembang. Topik lanjutan yang cocok ditambahkan berikutnya:

- `SOC.md`
- `DFIR.md`
- `Cloud-Security.md`
- `Application-Security.md`
- `Detection-Engineering.md`
- `Threat-Hunting.md`

Semua contoh command ditulis untuk pembelajaran dan lab yang sah. Gunakan hanya pada sistem yang kamu miliki atau yang memang kamu diberi izin untuk kelola.
