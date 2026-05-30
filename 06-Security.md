# Security

Sumber resmi utama:

- NIST Cybersecurity Framework: https://www.nist.gov/cyberframework
- NIST Computer Security Resource Center: https://csrc.nist.gov/
- NIST SP 800-207 Zero Trust Architecture: https://csrc.nist.gov/pubs/sp/800/207/final
- NIST National Vulnerability Database: https://nvd.nist.gov/
- CISA Resources and Tools: https://www.cisa.gov/resources-tools
- CISA Known Exploited Vulnerabilities Catalog: https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/

Area utama:

| Area | Cakupan |
|---|---|
| General Security Concepts | control, CIA, AAA, Zero Trust, change management, cryptography |
| Threats, Vulnerabilities, and Mitigations | threat actor, attack surface, malware, web/cloud/supply chain risk, mitigation |
| Security Architecture | secure infrastructure, cloud, data protection, resilience, IaC |
| Security Operations | hardening, IAM, vulnerability management, monitoring, IR, automation |
| Security Program Management and Oversight | governance, risk, third-party, compliance, audit, awareness |

## Daftar Isi

- [1.0 General Security Concepts](#10-general-security-concepts)
  - [1.1 Security Controls](#11-security-controls)
  - [1.2 CIA, AAA, Non-Repudiation, dan Zero Trust](#12-cia-aaa-non-repudiation-dan-zero-trust)
  - [1.3 Change Management and Security Impact](#13-change-management-and-security-impact)
  - [1.4 Cryptography, Hashing, PKI, and TLS](#14-cryptography-hashing-pki-and-tls)
  - [1.5 Security Principles in Daily Work](#15-security-principles-in-daily-work)
  - [1.6 Physical Security, Deception, and Disruption](#16-physical-security-deception-and-disruption)
- [2.0 Threats, Vulnerabilities, and Mitigations](#20-threats-vulnerabilities-and-mitigations)
  - [2.1 Threat Actors, Motivations, and Capability](#21-threat-actors-motivations-and-capability)
  - [2.2 Threat Vectors and Attack Surfaces](#22-threat-vectors-and-attack-surfaces)
  - [2.3 Malware, Social Engineering, and Password Attacks](#23-malware-social-engineering-and-password-attacks)
  - [2.4 Network, Application, Cloud, and Supply Chain Attacks](#24-network-application-cloud-and-supply-chain-attacks)
  - [2.5 Vulnerability Types and Root Causes](#25-vulnerability-types-and-root-causes)
  - [2.6 Mitigation Techniques](#26-mitigation-techniques)
- [3.0 Security Architecture](#30-security-architecture)
  - [3.1 Architecture Models](#31-architecture-models)
  - [3.2 Secure Network and Enterprise Infrastructure](#32-secure-network-and-enterprise-infrastructure)
  - [3.3 Cloud, Virtualization, Containers, IoT, and ICS](#33-cloud-virtualization-containers-iot-and-ics)
  - [3.4 Data Protection, Classification, and Privacy](#34-data-protection-classification-and-privacy)
  - [3.5 Resilience, Recovery, and Continuity](#35-resilience-recovery-and-continuity)
  - [3.6 Secure Application, IaC, and Cloud Control Patterns](#36-secure-application-iac-and-cloud-control-patterns)
- [4.0 Security Operations](#40-security-operations)
  - [4.1 Secure Baselines and Hardening](#41-secure-baselines-and-hardening)
  - [4.2 IAM, SSO, MFA, PAM, and Access Reviews](#42-iam-sso-mfa-pam-and-access-reviews)
  - [4.3 Asset Management](#43-asset-management)
  - [4.4 Vulnerability Management](#44-vulnerability-management)
  - [4.5 Monitoring, Logging, SIEM, SOAR, EDR, and XDR](#45-monitoring-logging-siem-soar-edr-and-xdr)
  - [4.6 Network and Endpoint Security Controls](#46-network-and-endpoint-security-controls)
  - [4.7 Incident Response and Digital Forensics](#47-incident-response-and-digital-forensics)
  - [4.8 Security Automation and Scripting](#48-security-automation-and-scripting)
  - [4.9 Mobile, Wireless, and Email Security Operations](#49-mobile-wireless-and-email-security-operations)
  - [4.10 Security Data Sources and Alert Triage](#410-security-data-sources-and-alert-triage)
- [5.0 Security Program Management and Oversight](#50-security-program-management-and-oversight)
  - [5.1 Governance, Policies, Standards, Procedures, and Baselines](#51-governance-policies-standards-procedures-and-baselines)
  - [5.2 Risk Management and Business Impact Analysis](#52-risk-management-and-business-impact-analysis)
  - [5.3 Third-Party Risk Management](#53-third-party-risk-management)
  - [5.4 Compliance, Privacy, and Data Governance](#54-compliance-privacy-and-data-governance)
  - [5.5 Audits, Assessments, and Penetration Testing Governance](#55-audits-assessments-and-penetration-testing-governance)
  - [5.6 Security Awareness and Human Risk](#56-security-awareness-and-human-risk)
- [6.0 Defensive Playbooks](#60-defensive-playbooks)
  - [6.1 Phishing Triage](#61-phishing-triage)
  - [6.2 Suspicious Endpoint Triage](#62-suspicious-endpoint-triage)
  - [6.3 Vulnerability Remediation Workflow](#63-vulnerability-remediation-workflow)
  - [6.4 Unauthorized Access Investigation](#64-unauthorized-access-investigation)
  - [6.5 Web Attack Triage](#65-web-attack-triage)
  - [6.6 Ransomware Readiness](#66-ransomware-readiness)

---

## 1.0 General Security Concepts

**Fokus teknis:** ubah konsep control dan risk menjadi mapping asset nyata.

```bash
# Tampilkan service listening yang memperbesar attack surface.
ss -tulpn

# Cek HTTP response header security dasar.
curl -I https://example.com/
```

Aspek teknis: asset, threat, vulnerability, preventive control, detective control, response control, dan data teknis yang mendukung risk statement.

### 1.1 Security Controls

Security control adalah mekanisme untuk mengurangi risiko. Control bisa mencegah, mendeteksi, memperbaiki, mengarahkan perilaku, atau mengompensasi kelemahan control lain.

Klasifikasi berdasarkan fungsi:

| Control | Arti | Contoh |
|---|---|---|
| Preventive | mencegah kejadian | MFA, firewall deny, least privilege |
| Detective | mendeteksi kejadian | SIEM alert, IDS, audit log |
| Corrective | memperbaiki setelah kejadian | restore backup, patch, reimage |
| Deterrent | membuat attacker/user enggan | warning banner, camera |
| Compensating | pengganti saat control utama tidak bisa | jump host saat legacy app tidak support MFA |
| Directive | mengarahkan perilaku | policy, standard, procedure |

Klasifikasi berdasarkan jenis:

| Jenis | Arti | Contoh |
|---|---|---|
| Technical | diterapkan lewat teknologi | EDR, encryption, ACL |
| Managerial | keputusan dan governance | risk register, policy approval |
| Operational | proses manusia/operasi | access review, change management |
| Physical | proteksi fisik | badge, lock, CCTV |

Contoh mapping risiko:

| Risiko | Preventive | Detective | Corrective |
|---|---|---|---|
| password compromise | MFA | impossible travel alert | reset password, revoke session |
| ransomware | application control | EDR behavior alert | restore backup, isolate host |
| data exfiltration | DLP, egress filtering | proxy/SIEM alert | revoke access, rotate keys |
| web attack | WAF, secure coding | web logs, IDS | patch app, block IP/rule |

```powershell
# Cek status firewall profile Windows sebagai preventive control.
Get-NetFirewallProfile

# Cek status Defender sebagai preventive dan detective control.
Get-MpComputerStatus

# Cek audit policy sebagai detective control.
auditpol /get /category:*
```

### 1.2 CIA, AAA, Non-Repudiation, dan Zero Trust

CIA triad adalah dasar tujuan security:

| Prinsip | Arti | Contoh kegagalan |
|---|---|---|
| Confidentiality | data hanya dilihat pihak berwenang | data customer bocor |
| Integrity | data tidak berubah tanpa izin | invoice dimodifikasi |
| Availability | sistem/data tersedia saat dibutuhkan | DNS/file server down |

AAA:

| Komponen | Arti | Contoh |
|---|---|---|
| Authentication | menunjukkan identitas subjek | password, certificate, MFA |
| Authorization | menentukan boleh akses apa | group, role, ACL |
| Accounting | mencatat aktivitas | audit log, SIEM |

Non-repudiation berarti pihak yang melakukan aksi tidak mudah menyangkal aksinya. Ini biasanya dibantu digital signature, audit log yang terlindungi, timestamp, dan identity yang kuat.

Zero Trust bukan satu produk. Zero Trust adalah model: jangan percaya hanya karena user/device ada di jaringan internal. Selalu verifikasi identity, device posture, context, dan privilege minimum.

Prinsip Zero Trust:

| Prinsip | Implementasi |
|---|---|
| verify explicitly | MFA, device compliance, risk-based access |
| least privilege | RBAC, JIT/JEA, scoped admin |
| assume breach | segmentation, monitoring, rapid containment |

```powershell
# Tampilkan group user saat ini untuk melihat authorization context.
whoami /groups

# Tampilkan privilege user saat ini.
whoami /priv

# Ambil event logon sukses untuk membaca accounting log.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4624} -MaxEvents 10
```

### 1.3 Change Management and Security Impact

Change management memastikan perubahan dilakukan dengan sadar risiko. Banyak incident security berasal dari perubahan kecil yang tidak dikontrol: firewall dibuka terlalu lebar, GPO salah scope, S3 bucket public, service account diberi admin, atau certificate expired.

Komponen change:

| Komponen | Fungsi |
|---|---|
| request | apa yang akan diubah |
| business reason | kenapa perubahan diperlukan |
| impact analysis | sistem/user/security apa yang terdampak |
| risk assessment | risiko teknis dan bisnis |
| approval | otorisasi perubahan |
| implementation plan | langkah eksekusi |
| rollback plan | cara kembali jika gagal |
| validation | indikator perubahan berhasil |
| post-change review | pembelajaran |

Jenis change:

| Jenis | Arti |
|---|---|
| standard | low-risk dan repeatable |
| normal | perlu review/approval |
| emergency | mendesak, tetap harus dicatat setelahnya |

Security impact yang harus dicek:

- apakah port baru terbuka
- apakah permission berubah
- apakah logging tetap aktif
- apakah encryption/certificate terdampak
- apakah backup/restore masih valid
- apakah attack surface bertambah
- apakah change bisa menjadi privilege escalation path

```powershell
# Export firewall rule sebelum perubahan.
Get-NetFirewallRule | Export-Csv C:\Temp\firewall-before.csv -NoTypeInformation

# Export local group Administrators sebelum perubahan.
Get-LocalGroupMember Administrators | Export-Csv C:\Temp\local-admins-before.csv -NoTypeInformation

# Ambil event perubahan service setelah change.
Get-WinEvent -FilterHashtable @{LogName="System"; Id=7040,7045} -MaxEvents 20
```

### 1.4 Cryptography, Hashing, PKI, and TLS

Cryptography dipakai untuk confidentiality, integrity, authentication, dan non-repudiation. Jangan hanya menghafal nama algoritma; pahami fungsi dan kapan dipakai.

Jenis crypto:

| Jenis | Fungsi | Contoh |
|---|---|---|
| Symmetric encryption | enkripsi cepat dengan satu shared key | AES |
| Asymmetric encryption | key pair public/private | RSA, ECC |
| Hashing | fingerprint satu arah | SHA-256 |
| HMAC | integrity + shared secret | HMAC-SHA256 |
| Digital signature | integrity, authentication, non-repudiation | RSA/ECDSA signature |
| Key exchange | membuat shared secret aman | ECDHE |

Hash bukan encryption. Hash tidak bisa didecrypt. Hash dipakai untuk integrity dan password storage dengan salt/KDF. Password sebaiknya tidak disimpan sebagai SHA biasa; gunakan KDF seperti bcrypt, scrypt, Argon2, atau PBKDF2 sesuai platform.

PKI:

| Komponen | Fungsi |
|---|---|
| CA | menerbitkan certificate |
| Root CA | trust anchor |
| Intermediate CA | CA perantara |
| Certificate | public key + identity + signature CA |
| Private key | rahasia pemilik certificate |
| CSR | request certificate |
| CRL/OCSP | revocation checking |
| EKU | tujuan certificate seperti Server Authentication |

TLS handshake secara konseptual:

1. Client meminta koneksi TLS.
2. Server mengirim certificate.
3. Client memvalidasi chain, hostname, expiry, dan trust.
4. Client dan server melakukan key exchange.
5. Keduanya membuat session key.
6. Traffic aplikasi dienkripsi dengan symmetric encryption.

```powershell
# Tampilkan certificate LocalMachine My store.
Get-ChildItem Cert:\LocalMachine\My

# Tampilkan detail certificate tertentu.
Get-ChildItem Cert:\LocalMachine\My | Select-Object Subject, Issuer, NotAfter, EnhancedKeyUsageList

# Test TLS/HTTPS endpoint dari PowerShell.
Test-NetConnection example.com -Port 443
```

```bash
# Tampilkan informasi certificate TLS dari server.
openssl s_client -connect example.com:443 -servername example.com

# Hitung hash SHA-256 sebuah file.
sha256sum file.iso

# Verifikasi fingerprint file dengan hash yang diketahui.
echo "EXPECTED_HASH  file.iso" | sha256sum -c -
```

### 1.5 Security Principles in Daily Work

Prinsip security harus terlihat dalam keputusan harian, bukan hanya dokumen.

Prinsip penting:

| Prinsip | Arti |
|---|---|
| least privilege | beri akses minimum yang diperlukan |
| need to know | akses data hanya jika perlu |
| separation of duties | tugas sensitif dipisah |
| defense in depth | beberapa lapis control |
| fail secure | saat gagal, default aman |
| secure by default | konfigurasi awal aman |
| economy of mechanism | desain sederhana lebih mudah diamankan |
| complete mediation | setiap akses diverifikasi |

Contoh:

| Situasi | Keputusan aman |
|---|---|
| admin butuh akses server | pakai admin account terpisah dan time-bound |
| vendor butuh akses | VPN/MFA/scope/logging/expiry |
| service butuh database | permission hanya DB dan action yang perlu |
| user butuh folder | akses via group, bukan ACL individual |

```powershell
# Tampilkan local administrators untuk review least privilege.
Get-LocalGroupMember Administrators

# Cari service yang berjalan dengan account domain.
Get-CimInstance Win32_Service | Where-Object StartName -like "*\\*" | Select-Object Name, StartName, State

# Cek scheduled task principal untuk menemukan privilege tersembunyi.
Get-ScheduledTask | Select-Object TaskName, TaskPath, State
```

### 1.6 Physical Security, Deception, and Disruption

Physical security melindungi sistem dari akses fisik tidak sah. Banyak control teknis bisa dilewati jika attacker punya akses fisik ke server, laptop, port jaringan, backup media, atau console hypervisor.

Physical controls:

| Control | Fungsi |
|---|---|
| badge/access card | membatasi akses area |
| biometric | verifikasi fisik user |
| mantrap | mencegah tailgating |
| lock/cabinet | melindungi device dan rack |
| CCTV | deterrent dan rekaman aktivitas |
| security guard | kontrol manusia |
| cable lock | mengurangi pencurian laptop |
| port blocker | mencegah akses port fisik |
| environmental control | suhu, listrik, fire suppression |

Serangan fisik:

| Serangan | Risiko |
|---|---|
| tailgating | orang tidak berwenang masuk area |
| shoulder surfing | melihat password/data |
| dumpster diving | mengambil dokumen/media |
| stolen device | akses data offline |
| rogue device | perangkat asing masuk network |
| evil twin | Wi-Fi palsu |

Deception dan disruption technology dipakai untuk mendeteksi atau mengganggu attacker. Tujuannya bukan mengganti hardening, tetapi menambah visibility dan memperlambat attacker.

| Teknologi | Fungsi |
|---|---|
| honeypot | sistem umpan yang terlihat menarik |
| honeynet | jaringan umpan |
| honeyfile | file umpan yang jika diakses memicu alert |
| honeytoken | credential/token palsu untuk deteksi misuse |
| sinkhole | mengarahkan domain/IP malicious ke sistem aman |
| tarpitting | memperlambat koneksi attacker/spam |

Hal yang harus dijaga:

- jangan membuat honeypot menjadi pivot ke jaringan produksi
- log deception harus masuk monitoring
- beri label dan segmentasi internal agar tim tidak salah mengira asset produksi
- honeytoken harus tidak punya privilege nyata

```powershell
# Cari akses file umpan jika object access auditing sudah aktif.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4663} -MaxEvents 50

# Cek device USB/storage yang pernah terdeteksi lewat event PnP.
Get-WinEvent -LogName "Microsoft-Windows-DriverFrameworks-UserMode/Operational" -MaxEvents 50
```

---

## 2.0 Threats, Vulnerabilities, and Mitigations

**Fokus teknis:** hubungkan threat dengan exposure dan mitigation yang bisa diuji.

```bash
# Scan port host milik sendiri untuk melihat exposure.
nmap -sV 127.0.0.1

# Melihat service listening lokal.
ss -tulpn

# Cari secret pattern sederhana di folder project.
rg -n "api_key|secret|password|token" .
```

Aspek teknis: exposed service, versi, kemungkinan vulnerability, mitigation, dan false positive yang perlu diverifikasi manual.

### 2.1 Threat Actors, Motivations, and Capability

Threat actor adalah pihak yang berpotensi menyerang atau menyalahgunakan sistem. Security engineer perlu menilai motivation, capability, opportunity, dan target.

Jenis threat actor:

| Actor | Motivasi | Capability umum |
|---|---|---|
| nation-state | espionage, disruption | tinggi, sabar, resource besar |
| organized crime | uang, extortion, fraud | sedang-tinggi |
| hacktivist | ideologi/politik | bervariasi |
| insider | balas dendam, uang, kelalaian | punya akses awal |
| script kiddie | reputasi, rasa ingin tahu | rendah-sedang |
| competitor | informasi bisnis | bervariasi |
| shadow IT | bukan attacker murni, tetapi membuat risiko | internal |

Motivasi:

| Motivasi | Contoh |
|---|---|
| financial gain | ransomware, fraud |
| espionage | pencurian IP/data |
| disruption | DDoS, sabotage |
| data exfiltration | customer data, source code |
| ideology | defacement, leak |
| insider convenience | bypass policy |

```powershell
# Cek perubahan user/group yang bisa menjadi indikator insider atau compromise.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4720,4726,4728,4732,4738} -MaxEvents 50

# Cek privileged logon yang perlu direview.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4672} -MaxEvents 50
```

### 2.2 Threat Vectors and Attack Surfaces

Threat vector adalah jalur serangan. Attack surface adalah semua titik yang bisa disentuh attacker.

Vector umum:

| Vector | Contoh |
|---|---|
| email/message | phishing, malicious attachment |
| web | vulnerable app, credential capture |
| network | exposed RDP, VPN, SMB |
| identity | password reuse, MFA fatigue |
| endpoint | malware, local privilege escalation |
| cloud | public bucket, exposed key |
| supply chain | dependency atau vendor compromise |
| physical | tailgating, stolen laptop |
| voice/SMS | vishing, smishing |

Attack surface harus diinventaris:

| Surface | Pertanyaan |
|---|---|
| external exposure | port/service apa terbuka ke internet |
| identity | account mana privileged |
| endpoint | device mana tidak compliant |
| cloud | resource mana public |
| application | endpoint/API mana sensitif |
| data | data sensitif tersimpan di mana |

```powershell
# Tampilkan port listening di Windows.
Get-NetTCPConnection -State Listen | Sort-Object LocalPort

# Tampilkan process pemilik port listening.
Get-NetTCPConnection -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess

# Cek koneksi ke port remote tertentu dari host yang diizinkan.
Test-NetConnection server.lab.local -Port 443
```

```bash
# Tampilkan port listening di Linux.
ss -tulpen

# Tampilkan service systemd yang aktif.
systemctl --type=service --state=running

# Cek koneksi TCP ke host dan port tertentu.
nc -vz server.lab.local 443
```

### 2.3 Malware, Social Engineering, and Password Attacks

Malware adalah software berbahaya. Social engineering menyerang manusia dan proses. Password attack menyerang authentication.

Jenis malware:

| Malware | Ciri |
|---|---|
| virus | menempel ke file/program |
| worm | menyebar sendiri |
| trojan | menyamar sebagai software sah |
| ransomware | mengenkripsi/mengunci data |
| spyware | memata-matai aktivitas |
| rootkit | menyembunyikan aktivitas |
| botnet agent | menerima command dari operator |
| fileless malware | memakai tool/memory bawaan |

Social engineering:

| Teknik | Contoh |
|---|---|
| phishing | email login palsu |
| spear phishing | target spesifik |
| whaling | target executive |
| vishing | voice call |
| smishing | SMS |
| pretexting | cerita palsu untuk mendapat info |
| baiting | USB/file menarik |
| tailgating | ikut masuk area fisik |

Password attack:

| Attack | Arti |
|---|---|
| brute force | coba banyak kombinasi |
| password spraying | satu password umum ke banyak user |
| credential stuffing | pakai credential bocor dari tempat lain |
| offline cracking | crack hash secara offline |
| keylogging | mencuri input |
| MFA fatigue | membanjiri prompt MFA |

```powershell
# Cari logon gagal Windows.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625} -MaxEvents 50

# Cari account lockout.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4740} -MaxEvents 20

# Cek detection Defender terbaru.
Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" -MaxEvents 30
```

```bash
# Cari gagal login SSH di systemd journal.
journalctl -u ssh --since "24 hours ago" | grep -i "failed"

# Cari sudo usage di auth log pada distro Debian/Ubuntu.
grep -i "sudo" /var/log/auth.log

# Tampilkan process mencurigakan dengan command line.
ps auxww
```

### 2.4 Network, Application, Cloud, and Supply Chain Attacks

Attack modern sering lintas layer: phishing mencuri credential, credential dipakai VPN, attacker bergerak lateral, lalu data dieksfiltrasi.

Network attacks:

| Attack | Arti | Defense |
|---|---|---|
| DDoS | membuat service tidak tersedia | DDoS protection, rate limit |
| MITM | menyisip di komunikasi | TLS, certificate validation |
| ARP spoofing | manipulasi LAN mapping | DHCP snooping, DAI |
| DNS poisoning | jawaban DNS palsu | DNSSEC untuk scenario tertentu, secure resolver |
| rogue DHCP | memberi IP/gateway/DNS salah | DHCP snooping |
| session hijacking | mengambil session | TLS, secure cookies |

Application attacks:

| Attack | Arti | Defense |
|---|---|---|
| SQL injection | input mengubah query SQL | parameterized query |
| XSS | script jalan di browser victim | output encoding, CSP |
| CSRF | aksi dipaksa dari browser victim | CSRF token, SameSite |
| SSRF | server dipaksa request internal | egress filter, metadata protection |
| path traversal | akses file di luar path | canonicalization, allowlist |
| insecure deserialization | object berbahaya diproses | avoid unsafe deserialize |

Cloud risks:

| Risiko | Contoh |
|---|---|
| public exposure | storage bucket public |
| credential leak | access key di repo |
| excessive IAM | wildcard permission |
| insecure security group | `0.0.0.0/0` ke admin port |
| no logging | CloudTrail/audit log mati |
| misconfigured KMS | key policy terlalu luas |

Supply chain:

| Risiko | Contoh |
|---|---|
| dependency compromise | package berbahaya |
| CI/CD secret leak | token di build log |
| vendor remote access | akses tanpa MFA |
| signed malware | certificate/code signing abuse |
| update channel compromise | update resmi disisipi malware |

```bash
# Cek HTTP response header security dasar.
curl -I https://example.com

# Cek DNS resolution domain.
dig example.com

# Cek certificate chain TLS.
openssl s_client -connect example.com:443 -servername example.com
```

### 2.5 Vulnerability Types and Root Causes

Vulnerability adalah kelemahan yang bisa dieksploitasi. Root cause sering lebih penting daripada nama CVE karena remediation jangka panjang bergantung pada akar masalah.

Jenis vulnerability:

| Area | Contoh |
|---|---|
| software | bug, insecure dependency |
| configuration | default password, exposed admin |
| identity | no MFA, weak password |
| network | flat network, open management port |
| cloud | public storage, permissive IAM |
| mobile | unmanaged device, sideloading |
| hardware | firmware bug, insecure boot |
| virtualization | exposed hypervisor, VM escape risk |
| web | injection, auth bypass |
| supply chain | compromised package/vendor |

Root cause:

| Root Cause | Contoh |
|---|---|
| missing asset inventory | server lama tidak dipatch |
| weak change control | firewall rule terlalu luas |
| no secure baseline | default insecure |
| poor secret management | key masuk repository |
| no monitoring | incident terlambat diketahui |
| excessive privilege | compromise berdampak besar |

```powershell
# Tampilkan hotfix Windows terbaru.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20

# Cek software yang terinstall via registry uninstall keys.
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher

# Cek local admin sebagai vulnerability privilege.
Get-LocalGroupMember Administrators
```

```bash
# Tampilkan package update yang tersedia di Debian/Ubuntu.
apt list --upgradable

# Tampilkan kernel Linux.
uname -a

# Tampilkan service listening yang memperbesar attack surface.
ss -tulpen
```

### 2.6 Mitigation Techniques

Mitigation mengurangi likelihood atau impact. Tidak semua vulnerability bisa langsung dihapus; kadang perlu compensating control sampai patch tersedia.

Teknik mitigation:

| Teknik | Fungsi |
|---|---|
| patching | memperbaiki vulnerability |
| hardening | mengurangi attack surface |
| segmentation | membatasi lateral movement |
| isolation | memisahkan sistem berisiko |
| access control | membatasi siapa bisa apa |
| allowlisting | hanya yang dikenal boleh jalan |
| configuration enforcement | baseline tetap konsisten |
| monitoring | mendeteksi exploit/abuse |
| backup | recovery dari data loss |
| encryption | mengurangi dampak data exposure |

Prioritas remediation:

| Faktor | Pertanyaan |
|---|---|
| exploitability | sudah ada exploit aktif |
| exposure | internet-facing atau internal |
| asset criticality | sistem penting bisnis |
| data sensitivity | ada data sensitif |
| compensating control | ada control sementara |
| operational risk | patch bisa memutus service |

```powershell
# Disable SMB1 sebagai hardening legacy protocol.
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart

# Aktifkan firewall profile Windows.
Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled True

# Buat rule allow management hanya dari subnet admin.
New-NetFirewallRule -DisplayName "Allow WinRM from Admin Subnet" -Direction Inbound -Protocol TCP -LocalPort 5985 -RemoteAddress 10.10.99.0/24 -Action Allow
```

```bash
# Cek status UFW firewall.
sudo ufw status verbose

# Izinkan SSH hanya dari subnet admin.
sudo ufw allow from 10.10.99.0/24 to any port 22 proto tcp

# Tampilkan service yang enabled saat boot.
systemctl list-unit-files --type=service --state=enabled
```

---

## 3.0 Security Architecture

**Fokus teknis:** baca arsitektur sebagai trust boundary, flow, dan control placement.

```bash
# Trace route untuk melihat path jaringan.
traceroute example.com

# Test TLS/HTTPS endpoint.
curl -Iv https://example.com/

# Capture DNS traffic untuk melihat dependency resolution.
sudo tcpdump -i eth0 -nn port 53
```

Aspek teknis: trust boundary, ingress/egress path, dependency DNS/TLS, segmentation, dan tempat control seharusnya berada.

### 3.1 Architecture Models

Security architecture adalah desain control dan trust boundary. Tujuannya membuat sistem tetap aman meskipun ada komponen gagal atau compromise.

Model umum:

| Model | Ciri |
|---|---|
| on-premises | hardware/network dikelola organisasi |
| cloud | shared responsibility dengan provider |
| hybrid | gabungan on-prem dan cloud |
| virtualization | workload di hypervisor |
| container | aplikasi terisolasi via image/runtime |
| serverless | function/event-driven, provider kelola runtime besar |
| IoT | device terbatas, sering sulit dipatch |
| ICS/OT | availability dan safety sangat kritis |

Trust boundary:

| Boundary | Contoh |
|---|---|
| internet to DMZ | reverse proxy, WAF |
| user to app | auth/session |
| app to database | service account, firewall |
| admin to server | jump host, PAM |
| cloud account/subscription | IAM boundary |
| production to development | data dan access isolation |

```powershell
# Tampilkan firewall profile untuk memahami boundary host.
Get-NetFirewallProfile

# Tampilkan route table Windows.
Get-NetRoute -AddressFamily IPv4

# Tampilkan DNS client untuk dependency name resolution.
Get-DnsClientServerAddress
```

### 3.2 Secure Network and Enterprise Infrastructure

Network security architecture mengatur zone, segmentation, ingress/egress, dan control point.

Komponen:

| Komponen | Fungsi |
|---|---|
| firewall | filter traffic |
| IDS | deteksi aktivitas mencurigakan |
| IPS | deteksi dan blokir |
| WAF | proteksi aplikasi web |
| proxy | kontrol web access |
| VPN | akses terenkripsi |
| NAC | kontrol device di jaringan |
| DNS filtering | blokir domain berbahaya |
| load balancer | distribusi traffic |
| reverse proxy | front-end service |

Zone umum:

| Zone | Isi |
|---|---|
| internet | untrusted external |
| DMZ | service publik terbatas |
| internal user | workstation/user subnet |
| server zone | workload internal |
| management zone | admin/jump/monitoring |
| identity zone | DC/IdP/PKI |
| backup zone | backup infrastructure |

Desain yang baik tidak hanya memblokir inbound. Egress control juga penting karena malware dan attacker butuh keluar untuk C2, download tool, atau exfiltration.

```bash
# Tampilkan route table Linux.
ip route

# Tampilkan rule firewall nftables jika dipakai.
sudo nft list ruleset

# Capture DNS traffic untuk troubleshooting defensive lab.
sudo tcpdump -n port 53
```

```powershell
# Tampilkan firewall rule inbound aktif.
Get-NetFirewallRule -Direction Inbound -Enabled True | Select-Object DisplayName, Profile, Action

# Test port ke service internal.
Test-NetConnection app01.lab.local -Port 443

# Resolve domain untuk memastikan DNS filtering/resolution.
Resolve-DnsName example.com
```

### 3.3 Cloud, Virtualization, Containers, IoT, and ICS

Cloud memakai shared responsibility model. Provider mengamankan sebagian stack, customer tetap bertanggung jawab atas identity, data, configuration, workload, dan access tergantung service model.

Shared responsibility:

| Model | Customer biasanya mengelola |
|---|---|
| IaaS | OS, app, data, IAM, network config |
| PaaS | app, data, IAM, sebagian config |
| SaaS | data, identity, access, configuration |

Virtualization risks:

| Risiko | Mitigasi |
|---|---|
| hypervisor admin terlalu luas | tiering, MFA, logging |
| snapshot berisi secret | encrypt, restrict access |
| VM sprawl | inventory dan lifecycle |
| management network exposed | isolate management |

Container risks:

| Risiko | Mitigasi |
|---|---|
| vulnerable image | image scanning |
| running as root | least privilege |
| secret in image | secret manager |
| overly permissive runtime | seccomp/AppArmor/SELinux |
| no registry control | signed/trusted images |

IoT/ICS:

| Area | Keterangan |
|---|---|
| IoT | device banyak, lifecycle panjang, patch sulit |
| ICS/OT | safety dan availability prioritas sangat tinggi |
| segmentation | pisahkan dari IT biasa |
| monitoring | passive monitoring sering lebih aman |
| patching | harus diuji karena downtime berdampak fisik |

```powershell
# Tampilkan VM Hyper-V pada host lab.
Get-VM

# Tampilkan virtual switch Hyper-V.
Get-VMSwitch

# Cek checkpoint VM agar tidak menumpuk.
Get-VMSnapshot -VMName "LAB-VM01"
```

```bash
# Tampilkan container yang berjalan.
docker ps

# Tampilkan image lokal untuk review lifecycle.
docker images

# Inspect container configuration.
docker inspect container_name
```

### 3.4 Data Protection, Classification, and Privacy

Data protection dimulai dari mengetahui data apa yang dimiliki. Tidak semua data sama. Data sensitif butuh control lebih kuat.

Klasifikasi umum:

| Label | Contoh |
|---|---|
| Public | press release |
| Internal | prosedur internal |
| Confidential | data customer, contract |
| Restricted | credential, key, regulated data |

Data state:

| State | Contoh control |
|---|---|
| at rest | disk/database encryption |
| in transit | TLS, VPN |
| in use | memory protection, access control |

Data control:

| Control | Fungsi |
|---|---|
| encryption | melindungi confidentiality |
| tokenization | mengganti data sensitif dengan token |
| masking | menyembunyikan sebagian data |
| DLP | mendeteksi/mencegah data keluar |
| rights management | kontrol akses dokumen |
| retention | durasi data disimpan |
| secure disposal | hapus data aman |

Privacy berbeda dari security. Security melindungi data. Privacy mengatur bagaimana data personal dikumpulkan, dipakai, dibagikan, dan dihapus.

```powershell
# Cek status BitLocker volume.
Get-BitLockerVolume

# Cari file besar yang mungkin perlu klasifikasi.
Get-ChildItem D:\Data -Recurse -File | Sort-Object Length -Descending | Select-Object -First 20 FullName, Length

# Tampilkan ACL folder data sensitif.
Get-Acl D:\Data\Sensitive | Format-List
```

```bash
# Cari file dengan permission world-writable.
find /srv/data -type f -perm -0002 -ls

# Tampilkan permission directory sensitif.
ls -la /srv/data

# Hitung hash file untuk integrity check.
sha256sum /srv/data/report.csv
```

### 3.5 Resilience, Recovery, and Continuity

Security juga mencakup availability dan recovery. Backup yang tidak pernah dites belum bisa dianggap backup.

Konsep:

| Konsep | Arti |
|---|---|
| BCP | Business Continuity Planning |
| DRP | Disaster Recovery Plan |
| RTO | waktu maksimal service boleh down |
| RPO | data loss maksimal |
| MTTR | waktu rata-rata pemulihan |
| MTBF | waktu rata-rata antar kegagalan |
| HA | high availability |
| failover | pindah ke sistem cadangan |
| tabletop exercise | latihan skenario |

Backup strategy:

| Prinsip | Arti |
|---|---|
| 3-2-1 | 3 copy, 2 media, 1 offsite |
| immutable backup | backup tidak bisa diubah attacker |
| offline copy | tidak selalu terkoneksi |
| restore test | validasi pemulihan |
| least privilege backup | backup admin tidak terlalu luas |

```powershell
# Cek VSS writers untuk backup consistency.
vssadmin list writers

# Tampilkan backup Windows Server jika tersedia.
wbadmin get versions

# Cek event backup terbaru.
Get-WinEvent -LogName "Microsoft-Windows-Backup" -MaxEvents 20
```

```bash
# Cek filesystem usage.
df -h

# Cek status service penting.
systemctl status nginx

# Test restore kecil dengan rsync dry-run.
rsync -avn /backup/sample/ /restore-test/sample/
```

### 3.6 Secure Application, IaC, and Cloud Control Patterns

Security architecture modern sering dibuat lewat code dan platform. Karena itu security harus masuk ke application design, CI/CD, Infrastructure as Code, dan cloud control plane.

Secure application principles:

| Area | Praktik |
|---|---|
| input validation | validasi input berdasarkan allowlist |
| output encoding | mencegah XSS |
| parameterized query | mencegah SQL injection |
| secure session | cookie `HttpOnly`, `Secure`, `SameSite` |
| secret management | secret tidak disimpan di source code |
| dependency management | dependency dipin, discan, dan diupdate |
| error handling | error tidak membocorkan detail sensitif |
| logging | log cukup tanpa menyimpan secret |

Security header umum:

| Header | Fungsi |
|---|---|
| `Content-Security-Policy` | membatasi sumber script/content |
| `Strict-Transport-Security` | memaksa HTTPS |
| `X-Content-Type-Options` | mengurangi MIME sniffing |
| `Referrer-Policy` | mengontrol referrer |
| `Permissions-Policy` | membatasi browser features |

IaC security:

| Risiko | Mitigasi |
|---|---|
| security group terlalu luas | policy-as-code dan review |
| secret di repository | secret scanning dan vault |
| drift manual | drift detection |
| module tidak terpercaya | source pinning dan review |
| destructive change | plan review dan approval |

Cloud security controls:

| Control | Fungsi |
|---|---|
| CASB | visibility dan policy untuk SaaS/cloud usage |
| CSPM | menemukan cloud misconfiguration |
| CWPP | melindungi workload cloud |
| CIEM | menganalisis entitlement/IAM cloud |
| SASE | gabungan network security dan access berbasis cloud |
| ZTNA | akses aplikasi berbasis identity/context |
| KMS/HSM | key management |

```bash
# Cek security header endpoint web.
curl -I https://example.com

# Cari secret pattern sederhana di repository lokal.
grep -RniE "(api_key|secret|password|token)" .

# Tampilkan dependency npm yang punya advisory jika memakai npm.
npm audit
```

```powershell
# Cari secret pattern sederhana di folder project dengan PowerShell.
Get-ChildItem -Recurse -File | Select-String -Pattern "api_key|secret|password|token"

# Test endpoint HTTPS aplikasi.
Test-NetConnection example.com -Port 443
```

---

## 4.0 Security Operations

**Fokus teknis:** lakukan operasi security dengan log, baseline, alert, dan data teknis.

```bash
# Melihat auth log Linux jika tersedia.
sudo tail -n 50 /var/log/auth.log

# Melihat process dan port listening.
ss -tulpn

# Melihat file yang berubah baru-baru ini di direktori kerja.
find . -type f -mtime -1 -ls
```

Aspek teknis: event time, actor, asset, action, source, destination, data teknis, dan keputusan triage.

### 4.1 Secure Baselines and Hardening

Baseline adalah konfigurasi standar yang dianggap aman dan operasional. Hardening mengurangi attack surface.

Area baseline:

| Area | Contoh |
|---|---|
| identity | MFA, password policy, lockout |
| endpoint | EDR, firewall, disk encryption |
| server | service minimum, patch, audit |
| network | segmentation, egress control |
| cloud | logging, IAM least privilege |
| application | secure headers, secret management |
| logging | central log, time sync |

Hardening harus diuji. Setting terlalu keras bisa memutus aplikasi, backup, monitoring, atau operasi admin.

```powershell
# Cek Windows Defender status.
Get-MpComputerStatus

# Cek firewall profile.
Get-NetFirewallProfile

# Cek SMB server configuration.
Get-SmbServerConfiguration

# Cek local password policy.
net accounts
```

```bash
# Tampilkan service enabled saat boot.
systemctl list-unit-files --type=service --state=enabled

# Cek SSH server configuration efektif jika sshd mendukung.
sudo sshd -T

# Tampilkan rule firewall UFW.
sudo ufw status numbered
```

### 4.2 IAM, SSO, MFA, PAM, and Access Reviews

IAM memastikan identity yang tepat mendapat akses yang tepat pada waktu yang tepat.

Konsep:

| Konsep | Arti |
|---|---|
| provisioning | membuat akses |
| deprovisioning | mencabut akses |
| SSO | single sign-on |
| MFA | multi-factor authentication |
| RBAC | role-based access control |
| ABAC | attribute-based access control |
| PAM | privileged access management |
| JIT | just-in-time access |
| JEA | just-enough administration |
| access review | review berkala akses |

Faktor authentication:

| Faktor | Contoh |
|---|---|
| something you know | password/PIN |
| something you have | hardware key, authenticator |
| something you are | biometric |
| somewhere you are | location context |
| something you do | behavior pattern |

Access review harus menjawab:

- siapa punya akses
- kenapa perlu akses
- kapan terakhir dipakai
- siapa owner approval
- kapan akses expired
- apakah akses terlalu luas

```powershell
# Tampilkan local administrators.
Get-LocalGroupMember Administrators

# Tampilkan AD privileged group members.
Get-ADGroupMember "Domain Admins"

# Cari user enabled yang password never expires.
Get-ADUser -Filter {Enabled -eq $true -and PasswordNeverExpires -eq $true} -Properties PasswordNeverExpires
```

### 4.3 Asset Management

Asset yang tidak diketahui tidak bisa diamankan. Asset management mencakup hardware, software, data, identity, cloud resource, certificate, dan dependency.

Jenis asset:

| Asset | Contoh |
|---|---|
| hardware | laptop, server, network device |
| software | aplikasi, library, package |
| data | database, file share, object storage |
| identity | user, group, service account |
| network | IP, subnet, domain, DNS |
| certificate | TLS cert, code signing cert |
| cloud | VM, storage, IAM role |

Lifecycle:

| Tahap | Security concern |
|---|---|
| acquisition | standard dan owner |
| deployment | baseline dan logging |
| operation | patching dan monitoring |
| transfer | ownership dan data |
| disposal | wipe, revoke, decommission |

```powershell
# Tampilkan hardware dan OS inventory Windows.
Get-ComputerInfo | Select-Object CsName, WindowsProductName, OsBuildNumber, CsManufacturer, CsModel

# Tampilkan installed hotfix.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20

# Tampilkan certificate yang akan expired dalam 60 hari.
Get-ChildItem Cert:\LocalMachine\My | Where-Object NotAfter -lt (Get-Date).AddDays(60) | Select-Object Subject, NotAfter
```

```bash
# Tampilkan informasi OS Linux.
cat /etc/os-release

# Tampilkan package yang terinstall pada Debian/Ubuntu.
dpkg -l

# Tampilkan certificate expiry remote.
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates
```

### 4.4 Vulnerability Management

Vulnerability management adalah proses berulang, bukan scan sekali.

Tahap:

| Tahap | Aktivitas |
|---|---|
| identify | scan, advisory, inventory |
| analyze | validate, remove false positive |
| prioritize | risk-based ranking |
| remediate | patch, config change, mitigation |
| validate | rescan/check |
| report | status, SLA, exception |

CVSS berguna, tetapi tidak cukup. Risk-based prioritization harus memasukkan exposure, exploit activity, asset criticality, dan control yang sudah ada.

Remediation outcome:

| Outcome | Arti |
|---|---|
| fixed | vulnerability hilang |
| mitigated | risiko dikurangi sementara |
| accepted | risiko diterima owner |
| false positive | temuan tidak valid |
| exception | diberi pengecualian terbatas waktu |

```powershell
# Cek update/hotfix Windows.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20

# Cek pending reboot CBS.
Test-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending"

# Cek software terinstall untuk inventory vulnerability.
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher
```

```bash
# Update metadata package Debian/Ubuntu.
sudo apt update

# Tampilkan package yang punya upgrade.
apt list --upgradable

# Cek reboot required pada Debian/Ubuntu.
test -f /var/run/reboot-required && cat /var/run/reboot-required
```

### 4.5 Monitoring, Logging, SIEM, SOAR, EDR, and XDR

Monitoring security mengubah aktivitas sistem menjadi data. Log harus punya timestamp benar, source jelas, dan retention cukup.

Tool kategori:

| Tool | Fungsi |
|---|---|
| SIEM | central log correlation dan alert |
| SOAR | automation response workflow |
| EDR | endpoint detection and response |
| XDR | correlation lintas endpoint/network/cloud/email |
| NDR | network detection and response |
| IDS/IPS | deteksi/blokir traffic |
| UEBA | behavior analytics user/entity |

Data source penting:

| Source | Contoh event |
|---|---|
| Windows Security | logon, group change, process |
| PowerShell | script block/module logging |
| Sysmon | process, network, file, registry |
| Linux auth | sudo, SSH, login |
| DNS | suspicious domain |
| proxy | URL/category/user |
| firewall | allow/deny |
| EDR | detection timeline |
| cloud audit | IAM/API changes |
| email security | phishing, attachment |

Event Windows penting:

| Event ID | Arti |
|---:|---|
| 4624 | logon sukses |
| 4625 | logon gagal |
| 4648 | explicit credentials |
| 4672 | special privileges |
| 4688 | process creation |
| 4697 | service installed |
| 4698 | scheduled task created |
| 4720 | user created |
| 4728 | member added to global group |
| 4732 | member added to local group |
| 4740 | account locked out |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket |
| 4771 | Kerberos pre-auth failed |
| 7045 | service installed |
| 1102 | audit log cleared |

```powershell
# Ambil logon gagal terbaru.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625} -MaxEvents 20

# Ambil event service installed.
Get-WinEvent -FilterHashtable @{LogName="System"; Id=7045} -MaxEvents 20

# Ambil PowerShell operational log.
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" -MaxEvents 30

# Export Security log untuk data teknis.
wevtutil epl Security C:\Temp\Security.evtx
```

```bash
# Tampilkan login SSH gagal.
journalctl -u ssh --since "24 hours ago" | grep -i "failed"

# Tampilkan sudo activity.
journalctl _COMM=sudo --since "24 hours ago"

# Tampilkan auth log terbaru pada Debian/Ubuntu.
sudo tail -n 100 /var/log/auth.log
```

### 4.6 Network and Endpoint Security Controls

Security operations mengelola control yang aktif di jaringan dan endpoint.

Network controls:

| Control | Fungsi |
|---|---|
| firewall | allow/deny traffic |
| IDS/IPS | detect/prevent malicious traffic |
| WAF | protect web app |
| DNS filtering | block domain malicious |
| secure web gateway | control web access |
| NAC | validate device before access |
| VPN/ZTNA | secure remote access |
| DLP | prevent data leakage |

Endpoint controls:

| Control | Fungsi |
|---|---|
| EDR | detect/respond endpoint behavior |
| host firewall | local network control |
| disk encryption | protect lost device |
| application control | block unapproved apps |
| patching | reduce vulnerability |
| device control | USB/peripheral policy |
| MDM | mobile/device policy |

```powershell
# Cek Defender real-time protection status.
Get-MpComputerStatus | Select-Object RealTimeProtectionEnabled, AntivirusEnabled, AMServiceEnabled

# Tampilkan firewall rule aktif.
Get-NetFirewallRule -Enabled True | Select-Object DisplayName, Direction, Action, Profile

# Cek BitLocker endpoint.
Get-BitLockerVolume
```

```bash
# Tampilkan listening port endpoint Linux.
ss -tulpen

# Tampilkan failed services.
systemctl --failed

# Cek AppArmor status jika tersedia.
sudo aa-status
```

### 4.7 Incident Response and Digital Forensics

Incident response adalah proses mengelola kejadian security dari deteksi sampai recovery dan lessons learned.

Lifecycle umum:

| Tahap | Aktivitas |
|---|---|
| preparation | playbook, logging, access, training |
| identification | validasi alert dan scope |
| containment | isolasi dan batasi dampak |
| eradication | hapus penyebab dan persistence |
| recovery | restore service aman |
| lessons learned | perbaiki control dan proses |

Forensics concepts:

| Konsep | Arti |
|---|---|
| data integrity | data tidak berubah |
| chain of custody | catatan siapa memegang data |
| timeline | urutan kejadian |
| volatile data | data hilang saat reboot |
| disk image | salinan forensic disk |
| memory capture | salinan memory |

Triage harus menjawab:

- apa alert-nya
- kapan mulai
- host/user mana terdampak
- apakah masih aktif
- apakah ada lateral movement
- data apa berisiko
- containment apa yang aman
- data apa harus disimpan

```powershell
# Tampilkan process berjalan untuk triage awal.
Get-Process | Sort-Object CPU -Descending | Select-Object -First 20 Name, Id, CPU, Path

# Tampilkan koneksi network aktif.
Get-NetTCPConnection | Where-Object State -eq Established | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess

# Tampilkan scheduled task yang aktif.
Get-ScheduledTask | Where-Object State -ne Disabled | Select-Object TaskName, TaskPath, State

# Ambil event log sekitar satu jam terakhir.
Get-WinEvent -FilterHashtable @{LogName="Security"; StartTime=(Get-Date).AddHours(-1)} -MaxEvents 200
```

```bash
# Tampilkan process dengan command line penuh.
ps auxww

# Tampilkan koneksi network aktif.
ss -tunap

# Tampilkan login terakhir.
last

# Tampilkan journal satu jam terakhir.
journalctl --since "1 hour ago"
```

### 4.8 Security Automation and Scripting

Automation membantu response cepat dan konsisten, tetapi automation juga bisa memperbesar kerusakan jika logic salah.

Use case:

| Use Case | Contoh |
|---|---|
| enrichment | lookup IP/domain/hash |
| containment | disable account, isolate endpoint |
| notification | buat ticket, kirim alert |
| log collection | export logs |
| compliance check | baseline drift |
| reporting | vulnerability SLA |

Risiko automation:

| Risiko | Mitigasi |
|---|---|
| false positive auto-containment | approval step untuk high impact |
| credential exposure | secret manager |
| destructive action | dry-run dan logging |
| rate limit/API error | retry/backoff |
| poor audit | semua action dicatat |

```powershell
# Contoh dry-run: tampilkan user disabled tanpa mengubah apa pun.
Search-ADAccount -AccountDisabled -UsersOnly

# Export event lockout untuk enrichment manual.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4740} -MaxEvents 50 | Export-Csv C:\Temp\lockouts.csv -NoTypeInformation

# Simpan hash file suspicious untuk lookup.
Get-FileHash C:\Temp\suspicious.bin -Algorithm SHA256
```

```bash
# Ambil hash file suspicious.
sha256sum suspicious.bin

# Ekstrak IP unik dari log web sederhana.
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head

# Cari status code error terbanyak dari access log.
awk '{print $9}' access.log | sort | uniq -c | sort -nr | head
```

### 4.9 Mobile, Wireless, and Email Security Operations

Mobile, wireless, dan email sering menjadi pintu awal incident karena langsung berhubungan dengan user, device, dan identity.

Mobile management:

| Konsep | Arti |
|---|---|
| MDM | Mobile Device Management untuk policy device |
| MAM | Mobile Application Management untuk app/data |
| BYOD | device milik user |
| CYOD | user memilih device dari daftar |
| COPE | company-owned personally enabled |
| COBO | company-owned business only |
| containerization | pemisahan data kerja dan pribadi |
| remote wipe | hapus data jarak jauh |

Mobile risks:

| Risiko | Mitigasi |
|---|---|
| device hilang | encryption, screen lock, remote wipe |
| app berbahaya | app protection policy |
| outdated OS | compliance policy |
| jailbroken/rooted | block non-compliant device |
| data leakage | MAM, DLP, copy/paste control |

Wireless security:

| Teknologi | Fungsi |
|---|---|
| WPA2/WPA3-Personal | password bersama |
| WPA2/WPA3-Enterprise | 802.1X/RADIUS |
| EAP-TLS | certificate-based authentication |
| captive portal | portal akses tamu |
| guest network | isolasi tamu dari internal |
| rogue AP detection | menemukan access point asing |

Email security:

| Control | Fungsi |
|---|---|
| SPF | host mana boleh kirim email domain |
| DKIM | signature email |
| DMARC | policy untuk SPF/DKIM alignment |
| secure email gateway | filtering attachment/URL |
| sandboxing | analisis attachment/link |
| user report button | memudahkan pelaporan phishing |

```powershell
# Query SPF record domain.
Resolve-DnsName example.com -Type TXT

# Query DMARC record domain.
Resolve-DnsName _dmarc.example.com -Type TXT

# Test koneksi ke RADIUS UDP tidak bisa divalidasi penuh dengan TCP, tetapi port umum perlu diketahui.
Get-NetUDPEndpoint | Where-Object LocalPort -in 1812,1813
```

```bash
# Query SPF/TXT record.
dig TXT example.com

# Query DMARC record.
dig TXT _dmarc.example.com

# Scan Wi-Fi sekitar pada Linux jika tool tersedia dan interface mendukung.
nmcli dev wifi list
```

### 4.10 Security Data Sources and Alert Triage

Alert yang baik harus bisa ditelusuri ke data source. Tanpa data source yang jelas, alert sulit divalidasi.

Data source dan kegunaan:

| Source | Berguna untuk |
|---|---|
| authentication logs | login, MFA, brute force |
| endpoint telemetry | process, file, registry, network |
| DNS logs | domain lookup dan C2 signal |
| proxy logs | URL, download, user browsing |
| firewall logs | allowed/blocked connection |
| VPN logs | remote access |
| cloud audit logs | API call dan IAM change |
| email logs | phishing delivery/click |
| application logs | auth, error, transaction |
| database logs | query dan admin activity |

Alert triage field:

| Field | Pertanyaan |
|---|---|
| who | user/account/service |
| what | action yang terjadi |
| when | waktu dan timezone |
| where | host, IP, geo, cloud account |
| how | protocol/tool/process |
| impact | data/service yang terdampak |
| confidence | true positive atau false positive |
| action | contain, monitor, close, escalate |

Severity tidak sama dengan priority:

| Istilah | Arti |
|---|---|
| severity | tingkat bahaya teknis |
| priority | urutan kerja berdasarkan severity, asset, exposure, dan business impact |

```powershell
# Ambil log security satu jam terakhir untuk triage.
Get-WinEvent -FilterHashtable @{LogName="Security"; StartTime=(Get-Date).AddHours(-1)} -MaxEvents 200

# Ambil DNS Client operational log jika aktif.
Get-WinEvent -LogName "Microsoft-Windows-DNS-Client/Operational" -MaxEvents 50

# Ambil Defender operational log.
Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" -MaxEvents 50
```

```bash
# Tampilkan journal satu jam terakhir.
journalctl --since "1 hour ago"

# Tampilkan auth log satu jam terakhir jika memakai journal.
journalctl _COMM=sshd --since "1 hour ago"

# Tampilkan Nginx access log terbaru.
sudo tail -n 100 /var/log/nginx/access.log
```

---

## 5.0 Security Program Management and Oversight

**Fokus teknis:** ubah governance menjadi artifact yang bisa diaudit.

```bash
# Cari file kebijakan atau baseline di repository.
rg -n "policy|standard|baseline|procedure|risk" .

# Cek file yang berubah untuk melihat scope perubahan.
git diff --stat
```

Aspek teknis: policy owner, control objective, data teknis yang harus dikumpulkan, exception, risk acceptance, dan review date.

### 5.1 Governance, Policies, Standards, Procedures, and Baselines

Governance memastikan security selaras dengan tujuan organisasi dan risiko bisnis.

Dokumen:

| Dokumen | Arti |
|---|---|
| policy | aturan tingkat tinggi |
| standard | requirement spesifik yang harus dipenuhi |
| procedure | langkah operasional |
| guideline | rekomendasi |
| baseline | konfigurasi minimum |
| playbook | langkah response scenario tertentu |

Contoh:

| Dokumen | Contoh |
|---|---|
| policy | semua admin wajib MFA |
| standard | password service account minimal 30 karakter |
| procedure | cara request privileged access |
| baseline | Windows Server hardening baseline |
| playbook | phishing triage playbook |

Kualitas dokumen security:

- owner jelas
- scope jelas
- versi dan tanggal review ada
- bisa dijalankan
- punya exception process
- selaras dengan risiko

### 5.2 Risk Management and Business Impact Analysis

Risk adalah kombinasi likelihood dan impact. Security engineer harus bisa menjelaskan risiko dalam bahasa teknis dan bisnis.

Rumus umum:

```text
Risk = Likelihood x Impact
```

Quantitative terms:

| Term | Arti |
|---|---|
| SLE | Single Loss Expectancy |
| ARO | Annualized Rate of Occurrence |
| ALE | Annualized Loss Expectancy |

```text
SLE = Asset Value x Exposure Factor
ALE = SLE x ARO
```

Risk response:

| Response | Arti |
|---|---|
| avoid | hentikan aktivitas berisiko |
| mitigate | kurangi likelihood/impact |
| transfer | pindahkan sebagian risiko, misalnya insurance |
| accept | terima risiko secara sadar |

BIA:

| Komponen | Arti |
|---|---|
| critical function | proses bisnis penting |
| dependency | sistem/vendor/team yang dibutuhkan |
| RTO | waktu pemulihan |
| RPO | toleransi kehilangan data |
| impact | finansial, legal, reputasi, operasional |

### 5.3 Third-Party Risk Management

Vendor bisa menjadi jalur risiko. Third-party risk bukan hanya kontrak; harus ada assessment, monitoring, dan offboarding.

Tahap vendor:

| Tahap | Security activity |
|---|---|
| selection | due diligence |
| contracting | security clauses, SLA, DPA |
| onboarding | access scope, MFA, logging |
| operation | monitoring, review, attestations |
| offboarding | revoke access, data return/delete |

Dokumen umum:

| Dokumen | Fungsi |
|---|---|
| NDA | kerahasiaan |
| MSA | master service terms |
| SLA | service level |
| DPA | data processing agreement |
| SOC 2 | assurance report |
| ISO 27001 certificate | ISMS certification |
| questionnaire | security assessment |
| ROE | rules of engagement untuk test |

Pertanyaan vendor:

- data apa yang diakses
- akses dari mana dan oleh siapa
- apakah MFA wajib
- bagaimana log dibagikan
- bagaimana incident diberitahukan
- bagaimana data dihapus saat kontrak selesai

### 5.4 Compliance, Privacy, and Data Governance

Compliance berarti memenuhi requirement eksternal/internal. Compliance tidak selalu berarti aman, tetapi bisa menjadi baseline minimum.

Area compliance:

| Area | Contoh |
|---|---|
| privacy | data personal |
| financial | audit finansial |
| healthcare | health data |
| payment card | cardholder data |
| government | public sector requirement |
| contractual | customer/vendor requirement |

Privacy principles:

| Prinsip | Arti |
|---|---|
| data minimization | kumpulkan yang perlu saja |
| purpose limitation | pakai sesuai tujuan |
| consent/legal basis | dasar pemrosesan jelas |
| retention | simpan sesuai kebutuhan |
| subject rights | akses/delete/correct sesuai aturan |
| breach notification | pemberitahuan saat insiden |

Data governance:

- data owner
- data steward
- classification
- retention
- access review
- lineage
- disposal

### 5.5 Audits, Assessments, and Penetration Testing Governance

Audit dan assessment membantu mengetahui apakah control berjalan. Penetration test harus punya scope dan authorization.

Jenis:

| Aktivitas | Fokus |
|---|---|
| internal audit | kepatuhan internal |
| external audit | assurance pihak luar |
| vulnerability assessment | menemukan vulnerability |
| penetration test | menunjukkan exploitability dalam scope |
| red team | menguji detection/response secara adversarial |
| tabletop exercise | latihan proses |
| control assessment | efektivitas control |

Pen test governance:

| Elemen | Arti |
|---|---|
| scope | target yang boleh diuji |
| ROE | aturan main |
| authorization | izin tertulis |
| timing | window test |
| safety | batasan destructive test |
| communication | escalation contact |
| reporting | format temuan dan severity |

### 5.6 Security Awareness and Human Risk

User bukan "masalah"; user adalah bagian dari sistem. Awareness yang baik membuat orang tahu cara mengambil keputusan aman dan melaporkan hal mencurigakan.

Materi awareness:

| Topik | Tujuan |
|---|---|
| phishing | mengenali dan melapor |
| password/MFA | melindungi account |
| data handling | klasifikasi dan sharing |
| social engineering | verifikasi request |
| removable media | risiko USB |
| travel security | Wi-Fi, device loss |
| incident reporting | kapan dan ke mana lapor |

Metric awareness:

| Metric | Keterangan |
|---|---|
| report rate | user melaporkan phishing |
| click rate | indikator risiko, jangan jadi shame tool |
| training completion | baseline kepatuhan |
| repeat risky behavior | butuh coaching |
| time to report | penting untuk containment |

---

## 6.0 Defensive Playbooks

**Fokus teknis:** jalankan playbook seperti investigasi kecil dengan timeline.

```bash
# Tampilkan IP terbanyak dari access log.
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head

# Cari pattern path traversal sederhana.
grep -Ei '(\.\./|%2e%2e|%252e)' access.log
```

Aspek teknis: alert source, first seen, last seen, impacted asset, data teknis, containment, eradication, recovery, dan lesson learned.

### 6.1 Phishing Triage

Tujuan phishing triage adalah menentukan apakah email berbahaya, siapa terdampak, apakah credential/data sudah bocor, dan tindakan containment.

Data teknis:

| Data teknis | Contoh |
|---|---|
| header | SPF, DKIM, DMARC, sender path |
| URL | domain, redirect, reputation |
| attachment | hash, macro, file type |
| recipient | siapa menerima/membuka |
| click logs | proxy/browser/email security |
| auth logs | login setelah klik |

Langkah:

1. Ambil sample email/header.
2. Extract URL/attachment hash.
3. Cek siapa menerima.
4. Cek siapa klik atau submit credential.
5. Block sender/domain/URL/hash jika valid.
6. Revoke session dan reset password jika credential terdampak.
7. Hunt email serupa.

```powershell
# Cari explicit credential logon setelah waktu phishing.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4648; StartTime=(Get-Date).AddHours(-6)} -MaxEvents 50

# Cari logon gagal yang mungkin terkait password spraying setelah phishing.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625; StartTime=(Get-Date).AddHours(-6)} -MaxEvents 50
```

```bash
# Extract URL sederhana dari file email mentah.
grep -Eo 'https?://[^ ]+' suspicious-email.eml

# Hitung hash attachment.
sha256sum attachment.bin
```

### 6.2 Suspicious Endpoint Triage

Endpoint triage mencari tanda compromise pada process, network, persistence, user activity, dan security tool status.

Data teknis:

| Area | Cek |
|---|---|
| process | nama, path, parent, command line |
| network | remote IP/domain/port |
| persistence | service, scheduled task, startup |
| user | logon, privilege, recent activity |
| files | recent created/modified |
| security tool | Defender/EDR status |
| logs | Security, Sysmon, PowerShell |

```powershell
# Tampilkan process dengan path.
Get-Process | Select-Object Name, Id, Path | Sort-Object Name

# Tampilkan koneksi established.
Get-NetTCPConnection -State Established | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess

# Tampilkan scheduled task non-disabled.
Get-ScheduledTask | Where-Object State -ne Disabled | Select-Object TaskName, TaskPath, State

# Tampilkan service yang auto start.
Get-CimInstance Win32_Service | Where-Object StartMode -eq "Auto" | Select-Object Name, State, StartName, PathName
```

```bash
# Tampilkan process lengkap.
ps auxww

# Tampilkan koneksi dengan process.
sudo ss -tunap

# Tampilkan cron user root.
sudo crontab -l

# Tampilkan systemd service enabled.
systemctl list-unit-files --type=service --state=enabled
```

### 6.3 Vulnerability Remediation Workflow

Vulnerability remediation harus punya SLA dan exception process.

Workflow:

1. Validate finding.
2. Identify asset owner.
3. Determine exposure.
4. Check exploit activity.
5. Choose remediation or mitigation.
6. Schedule change.
7. Apply fix.
8. Validate.
9. Document closure.

SLA contoh:

| Severity | Target |
|---|---|
| critical internet-facing | 24-72 jam |
| critical internal | 7-14 hari |
| high | 14-30 hari |
| medium | 30-60 hari |
| low | sesuai cycle |

```powershell
# Export installed hotfix untuk dokumentasi remediation.
Get-HotFix | Sort-Object InstalledOn -Descending | Export-Csv C:\Temp\hotfix.csv -NoTypeInformation

# Cek pending reboot setelah patch.
Test-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending"
```

```bash
# Tampilkan package upgrade yang tersisa setelah patching.
apt list --upgradable

# Cek apakah reboot diperlukan.
test -f /var/run/reboot-required && cat /var/run/reboot-required
```

### 6.4 Unauthorized Access Investigation

Unauthorized access investigation fokus pada identity, source, session, privilege, dan action.

Pertanyaan:

- account apa
- login dari mana
- MFA berhasil atau tidak
- privilege apa yang dipakai
- resource apa yang diakses
- ada perubahan group/password/session
- masih aktif atau sudah berhenti

```powershell
# Cari logon sukses terbaru.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4624} -MaxEvents 50

# Cari logon gagal terbaru.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625} -MaxEvents 50

# Cari perubahan group membership.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4728,4732,4756} -MaxEvents 50

# Cari special privileges assigned.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4672} -MaxEvents 50
```

```bash
# Tampilkan login history.
last

# Tampilkan failed login history jika tersedia.
lastb

# Cari accepted SSH login.
journalctl -u ssh --since "24 hours ago" | grep -i "accepted"
```

### 6.5 Web Attack Triage

Web attack triage mencari request mencurigakan, source, target endpoint, status code, user-agent, dan dampak.

Indikator:

| Indikator | Contoh |
|---|---|
| SQLi pattern | `' OR 1=1`, `UNION SELECT` |
| XSS pattern | `<script>`, event handler |
| traversal | `../`, encoded path |
| scanning | banyak 404/403 |
| brute force | banyak POST login |
| SSRF attempt | metadata IP/cloud endpoint |

```bash
# Tampilkan IP terbanyak dari access log.
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head

# Tampilkan status code terbanyak.
awk '{print $9}' access.log | sort | uniq -c | sort -nr | head

# Cari pattern path traversal sederhana.
grep -Ei '(\.\./|%2e%2e|%252e)' access.log

# Cari pattern SQL injection sederhana.
grep -Ei "(union select|or 1=1|sleep\\(|benchmark\\()" access.log
```

```powershell
# Tampilkan log IIS terbaru.
Get-ChildItem "C:\inetpub\logs\LogFiles" -Recurse -Filter "*.log" | Sort-Object LastWriteTime -Descending | Select-Object -First 5

# Cari status 500 di log IIS.
Select-String -Path "C:\inetpub\logs\LogFiles\*\*.log" -Pattern " 500 "
```

### 6.6 Ransomware Readiness

Ransomware defense bukan hanya EDR. Perlu identity hardening, segmentation, backup, patching, least privilege, monitoring, dan restore exercise.

Readiness checklist:

| Area | Control |
|---|---|
| identity | MFA, admin tiering, no shared admin |
| endpoint | EDR, application control |
| network | segmentation, restrict SMB/RDP |
| backup | immutable/offline, restore tested |
| email | attachment/URL filtering |
| vulnerability | patch internet-facing systems |
| monitoring | mass file change, service install, suspicious PowerShell |
| response | isolate hosts, disable accounts, legal/comms plan |

Early warning:

| Signal | Event/Source |
|---|---|
| many file renames | file server audit/EDR |
| service installed | 7045 |
| scheduled task created | 4698 |
| admin share access | Windows logs/SIEM |
| backup deletion | backup platform logs |
| shadow copy deletion | command/process telemetry |

```powershell
# Cari service installed yang bisa menjadi persistence.
Get-WinEvent -FilterHashtable @{LogName="System"; Id=7045} -MaxEvents 50

# Cari scheduled task created.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4698} -MaxEvents 50

# Cek shadow copies.
vssadmin list shadows

# Cek backup versions.
wbadmin get versions
```

---
