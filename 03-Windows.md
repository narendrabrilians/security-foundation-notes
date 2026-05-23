# Windows

Catatan Windows pribadi untuk belajar serius, praktik lab, dan referensi kerja sebagai system administrator.

Fokus halaman ini adalah Windows client dan administrasi host. Active Directory dibahas hanya sebagai konteks singkat; pembahasan domain controller, GPO, Kerberos domain, replication, FSMO, dan AD security sebaiknya dibuat di `Active-Directory.md` setelah dasar Windows dan Windows Server matang.

Area utama:

| Area | Cakupan |
|---|---|
| Fundamentals | edition, workgroup/domain, boot, registry, services, filesystem |
| Administration | local user/group, NTFS/share permission, storage, apps, updates, drivers |
| Networking | IP config, DNS client, firewall, SMB, remote management |
| Security | UAC, Defender, BitLocker, credential, audit, hardening |
| PowerShell | object pipeline, modules, remoting, scripting, automation |
| Troubleshooting | Event Viewer, performance, boot recovery, network, logs |

## Daftar Isi

- [0. Cara Pakai Catatan Ini](#0-cara-pakai-catatan-ini)
- [1.0 Windows Fundamentals](#10-windows-fundamentals)
  - [1.1 Windows Client, Server, Workgroup, dan Domain](#11-windows-client-server-workgroup-dan-domain)
  - [1.2 Windows Architecture dan Boot](#12-windows-architecture-dan-boot)
  - [1.3 Filesystem, Path, dan Environment](#13-filesystem-path-dan-environment)
  - [1.4 Registry](#14-registry)
  - [1.5 Processes, Services, dan Scheduled Tasks](#15-processes-services-dan-scheduled-tasks)
- [2.0 System Administration](#20-system-administration)
  - [2.1 Local Users and Groups](#21-local-users-and-groups)
  - [2.2 NTFS Permission, Share Permission, dan UAC](#22-ntfs-permission-share-permission-dan-uac)
  - [2.3 Storage, Disk, Volume, dan BitLocker](#23-storage-disk-volume-dan-bitlocker)
  - [2.4 Apps, Packages, dan Windows Update](#24-apps-packages-dan-windows-update)
  - [2.5 Devices and Drivers](#25-devices-and-drivers)
- [3.0 Windows Networking](#30-windows-networking)
  - [3.1 TCP/IP Configuration](#31-tcpip-configuration)
  - [3.2 DNS Client, Hosts File, dan Name Resolution](#32-dns-client-hosts-file-dan-name-resolution)
  - [3.3 Windows Firewall and Remote Access](#33-windows-firewall-and-remote-access)
  - [3.4 SMB and File Sharing](#34-smb-and-file-sharing)
- [4.0 Windows Security](#40-windows-security)
  - [4.1 Authentication, Tokens, dan Credential](#41-authentication-tokens-dan-credential)
  - [4.2 Defender, Firewall, dan Attack Surface Reduction](#42-defender-firewall-dan-attack-surface-reduction)
  - [4.3 Logging, Auditing, dan Event Viewer](#43-logging-auditing-dan-event-viewer)
  - [4.4 Hardening Baseline](#44-hardening-baseline)
- [5.0 PowerShell](#50-powershell)
  - [5.1 Cmdlet, Object, Pipeline, dan Help](#51-cmdlet-object-pipeline-dan-help)
  - [5.2 Modules, Execution Policy, dan Profiles](#52-modules-execution-policy-dan-profiles)
  - [5.3 PowerShell Remoting](#53-powershell-remoting)
  - [5.4 Scripting Basics](#54-scripting-basics)
- [6.0 Troubleshooting and Operations](#60-troubleshooting-and-operations)
  - [6.1 Event Logs and Evidence](#61-event-logs-and-evidence)
  - [6.2 Performance and Resource Troubleshooting](#62-performance-and-resource-troubleshooting)
  - [6.3 Boot, Recovery, dan Repair](#63-boot-recovery-dan-repair)
  - [6.4 Network Troubleshooting](#64-network-troubleshooting)
- [7.0 Senior Deep Dive](#70-senior-deep-dive)
  - [7.1 Windows Internals Overview](#71-windows-internals-overview)
  - [7.2 Services, Sessions, Handles, dan Tokens](#72-services-sessions-handles-dan-tokens)
  - [7.3 ETW, Sysmon, dan Sysinternals](#73-etw-sysmon-dan-sysinternals)
  - [7.4 Operational Runbook](#74-operational-runbook)
  - [7.5 Security Descriptor, ACL, ACE, dan Integrity Level](#75-security-descriptor-acl-ace-dan-integrity-level)
  - [7.6 LSASS, Authentication Flow, dan Credential Protection](#76-lsass-authentication-flow-dan-credential-protection)
  - [7.7 Windows Servicing, Component Store, dan Image Health](#77-windows-servicing-component-store-dan-image-health)
  - [7.8 WMI, CIM, WinRM, dan Remote Operations](#78-wmi-cim-winrm-dan-remote-operations)
  - [7.9 Crash Dump, Reliability, dan Driver Failure](#79-crash-dump-reliability-dan-driver-failure)
  - [7.10 Expert Troubleshooting Playbooks](#710-expert-troubleshooting-playbooks)
- [Lab Checklist](#lab-checklist)
- [Command Index Cepat](#command-index-cepat)
- [Checklist Kesiapan Praktik](#checklist-kesiapan-praktik)

---

## 0. Cara Pakai Catatan Ini

Saat belajar atau bekerja dengan Windows, jangan hanya menghafal menu GUI. Biasakan menjawab:

- masalah terjadi di user profile, service, driver, network, storage, security, atau aplikasi
- command apa yang membuktikan kondisi sistem
- event log mana yang relevan
- perubahan mana yang persistent dan mana yang sementara
- apakah masalah hanya terjadi pada satu user, satu komputer, atau semua komputer
- apakah host standalone, workgroup, domain-joined, atau hybrid-managed

Format command:

```powershell
# Setiap command diberi komentar singkat.
Get-Command
```

Lab minimum:

- satu VM Windows 11
- satu VM Windows Server untuk tahap berikutnya
- snapshot sebelum eksperimen registry, service, permission, dan firewall
- akses PowerShell sebagai user biasa dan administrator
- tool bawaan: Event Viewer, Task Manager, Resource Monitor, Services, Windows Terminal

---

## 1.0 Windows Fundamentals

### 1.1 Windows Client, Server, Workgroup, dan Domain

Windows client dan Windows Server memakai keluarga teknologi yang sama, tetapi role-nya berbeda.

Windows client seperti Windows 10/11 dirancang untuk endpoint user: laptop, desktop, workstation, atau VM yang dipakai manusia untuk bekerja. Fokusnya adalah pengalaman user, aplikasi desktop, device seperti printer/Wi-Fi/Bluetooth, dan keamanan endpoint. Windows Server memakai kernel dan banyak komponen yang masih satu keluarga, tetapi default behavior dan fiturnya diarahkan untuk menjalankan service yang dipakai banyak client, misalnya file sharing, DNS, DHCP, Hyper-V, IIS, Remote Desktop Services, atau Active Directory Domain Services.

Hal yang sering membingungkan: "Windows" bukan selalu berarti Active Directory. Satu laptop Windows bisa berdiri sendiri tanpa domain. Active Directory baru masuk ketika organisasi butuh identity pusat, policy pusat, dan komputer-komputer yang dikelola sebagai satu domain.

| Item | Fungsi |
|---|---|
| Windows 10/11 | OS client untuk user endpoint |
| Windows Server | OS server untuk role seperti file server, DNS, DHCP, Hyper-V, AD DS |
| Workgroup | komputer berdiri sendiri tanpa identity pusat |
| Domain | komputer bergabung ke directory pusat seperti Active Directory |
| Local account | account hanya ada di komputer itu |
| Microsoft account | account consumer/cloud untuk Windows/client services |
| Domain account | account dikelola Active Directory |
| Entra ID account | identity cloud Microsoft Entra |

Contoh situasi:

| Situasi | Model |
|---|---|
| Laptop pribadi login dengan local user | standalone/workgroup |
| PC kantor login dengan `COMPANY\alice` | domain-joined |
| Laptop modern login dengan akun kerja Microsoft 365 | Entra ID joined/hybrid |
| File server kantor memberi share `\\fileserver\data` | Windows Server role |
| Domain controller mengatur login dan GPO | Active Directory |

Urutan belajar:

1. Windows client dan local administration.
2. Windows Server basics.
3. DNS, DHCP, SMB, remote management.
4. Active Directory.
5. Group Policy dan AD security.

Workgroup vs domain:

| Area | Workgroup | Domain |
|---|---|---|
| Identity | local per komputer | centralized |
| Login | local account | domain account |
| Policy | local policy | Group Policy |
| Scale | kecil/lab/home | organisasi |
| Admin | per host | centralized delegation |

Kenapa urutannya Windows dulu:

- Kamu perlu paham local user sebelum domain user.
- Kamu perlu paham local policy sebelum Group Policy.
- Kamu perlu paham NTFS permission sebelum file server permission.
- Kamu perlu paham DNS client sebelum AD DNS.
- Kamu perlu paham Event Viewer sebelum membaca event domain controller.

Kalau langsung masuk AD tanpa dasar Windows, banyak error akan terasa abstrak. Contohnya, user gagal login domain bisa disebabkan DNS, time sync, password, locked account, network, cached credential, secure channel, atau Kerberos. Dasar Windows membantu memisahkan kemungkinan itu.

Command identitas host:

```powershell
# Melihat nama komputer.
hostname

# Melihat informasi komputer dan OS.
Get-ComputerInfo

# Melihat apakah komputer join domain atau workgroup.
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain
```

### 1.2 Windows Architecture dan Boot

Windows memisahkan user mode dan kernel mode.

Pemisahan ini penting untuk stabilitas dan keamanan. Aplikasi biasa berjalan di user mode, sehingga kalau aplikasi crash, biasanya hanya aplikasinya yang mati. Kernel mode punya akses jauh lebih dalam ke memory, hardware, dan driver. Kalau driver kernel bermasalah, dampaknya bisa lebih berat: blue screen, boot loop, atau sistem hang.

Cara membacanya saat troubleshooting:

- Aplikasi crash berulang biasanya mulai dari Application log, process, dependency, atau profile user.
- Driver crash, disk error, service boot-start, dan hardware issue biasanya terlihat di System log.
- Security/login/token biasanya berkaitan dengan LSASS, Security log, UAC, policy, atau credential.

| Layer | Arti | Contoh |
|---|---|---|
| User mode | proses aplikasi berjalan dengan isolasi | browser, PowerShell, services user-mode |
| Kernel mode | akses rendah ke hardware dan kernel | kernel, drivers |
| HAL | hardware abstraction layer | abstraksi hardware |
| Win32 subsystem | API utama aplikasi Windows klasik | process, file, registry API |
| Service Control Manager | mengelola services | start/stop service |
| LSASS | authentication/security authority | logon, token, credential policy |

User mode vs kernel mode:

| Area | User Mode | Kernel Mode |
|---|---|---|
| Akses hardware | tidak langsung | langsung lewat driver/kernel |
| Crash impact | biasanya hanya process/app | bisa BSOD/system-wide |
| Contoh | browser, Notepad, PowerShell | storage driver, network driver |
| Debug evidence | Application log, dump app | memory dump, BugCheck, System log |

Boot flow modern:

```text
UEFI/BIOS
-> Windows Boot Manager
-> BCD store
-> Windows OS Loader
-> kernel + HAL + boot drivers
-> Session Manager
-> services
-> logon screen
```

Penjelasan alur boot:

1. Firmware UEFI/BIOS menemukan boot entry.
2. Windows Boot Manager membaca BCD untuk tahu OS mana yang akan dimuat.
3. OS Loader memuat kernel, HAL, dan driver yang dibutuhkan saat boot.
4. Kernel mulai membuat environment dasar Windows.
5. Session Manager menjalankan tahap awal user-mode.
6. Service Control Manager mulai menjalankan services.
7. Logon UI muncul agar user bisa masuk.

Jika boot gagal, lokasi masalah biasanya bisa dipersempit dari tahap ini. Misalnya BCD rusak membuat OS tidak ditemukan, driver storage rusak bisa membuat boot crash, sedangkan service rusak bisa membuat login lambat atau system hang setelah boot.

Komponen boot:

| Komponen | Fungsi |
|---|---|
| EFI System Partition | menyimpan boot loader UEFI |
| BCD | Boot Configuration Data |
| winload.efi | Windows OS loader |
| ntoskrnl.exe | Windows kernel |
| boot-start drivers | driver penting saat boot |
| WinRE | Windows Recovery Environment |

Command boot/system:

```powershell
# Melihat konfigurasi boot BCD.
bcdedit

# Melihat uptime dan boot time.
Get-CimInstance Win32_OperatingSystem | Select-Object LastBootUpTime

# Melihat informasi BIOS/UEFI.
Get-CimInstance Win32_BIOS
```

### 1.3 Filesystem, Path, dan Environment

Windows memakai drive letter dan path dengan backslash.

Drive letter seperti `C:` adalah cara Windows menampilkan volume ke user. Di bawahnya tetap ada disk, partition, volume, filesystem, dan mount point. Satu komputer bisa punya beberapa volume seperti `C:`, `D:`, atau volume tanpa drive letter yang dipasang ke folder tertentu.

Path Windows juga punya beberapa hal khas:

- `\` dipakai sebagai separator folder.
- Path bisa memakai environment variable seperti `%USERPROFILE%`.
- UNC path seperti `\\server\share` dipakai untuk akses network share.
- Beberapa folder terlihat mirip tetapi punya makna berbeda, misalnya `System32` dan `SysWOW64`.

Contoh:

```text
C:\Windows\System32\drivers\etc\hosts
```

Path penting:

| Path | Fungsi |
|---|---|
| `C:\Windows` | direktori OS |
| `C:\Windows\System32` | binary dan library sistem 64-bit |
| `C:\Windows\SysWOW64` | komponen 32-bit pada Windows 64-bit |
| `C:\Users\<user>` | profile user |
| `C:\Program Files` | aplikasi 64-bit |
| `C:\Program Files (x86)` | aplikasi 32-bit |
| `C:\ProgramData` | data aplikasi shared |
| `%TEMP%` | temporary files user |

Environment variable:

| Variable | Arti |
|---|---|
| `%USERPROFILE%` | home/profile user |
| `%APPDATA%` | roaming app data |
| `%LOCALAPPDATA%` | local app data |
| `%PROGRAMDATA%` | shared application data |
| `%WINDIR%` | direktori Windows |
| `%PATH%` | daftar lokasi executable |

Profile user:

| Folder | Fungsi |
|---|---|
| `Desktop` | file yang terlihat di desktop |
| `Documents` | dokumen user |
| `Downloads` | file unduhan |
| `AppData\Roaming` | setting aplikasi yang bisa roam antar komputer domain |
| `AppData\Local` | data aplikasi lokal komputer |
| `AppData\LocalLow` | data aplikasi low integrity/sandbox tertentu |

`System32` vs `SysWOW64`:

| Path | Isi |
|---|---|
| `C:\Windows\System32` | binary sistem 64-bit pada Windows 64-bit |
| `C:\Windows\SysWOW64` | binary 32-bit pada Windows 64-bit |

Namanya memang membingungkan. Di Windows 64-bit, `System32` tetap berisi komponen 64-bit karena alasan compatibility lama.

NTFS feature:

| Feature | Fungsi |
|---|---|
| ACL | access control list |
| Alternate Data Streams | stream tambahan pada file |
| EFS | file-level encryption lama |
| Compression | kompresi file/folder |
| Quota | batas penggunaan disk |
| Reparse point | mount point, junction, symlink |

File attribute:

| Attribute | Arti |
|---|---|
| Read-only | file tidak mudah ditulis |
| Hidden | disembunyikan dari tampilan biasa |
| System | file sistem |
| Archive | penanda backup/archive |

Reparse point:

| Jenis | Arti |
|---|---|
| Junction | link folder gaya Windows lama |
| Symbolic link | pointer ke file/folder lain |
| Mount point | volume dipasang ke folder |

Reparse point berguna, tetapi bisa membingungkan backup, scanning, dan script recursive. Saat script berjalan recursive, pastikan tidak mengikuti link yang membuat loop.

Command filesystem:

```powershell
# Melihat isi folder termasuk hidden file.
Get-ChildItem -Force

# Melihat environment variable.
Get-ChildItem Env:

# Melihat lokasi executable dari command.
Get-Command notepad

# Melihat ACL file atau folder.
Get-Acl C:\Windows

# Melihat item dengan attribute tertentu.
Get-ChildItem C:\ -Force | Select-Object Name, Attributes
```

### 1.4 Registry

Registry adalah database konfigurasi Windows. Banyak setting OS, aplikasi, service, driver, dan policy tersimpan di sini.

Bayangkan registry sebagai database hierarkis seperti filesystem, tetapi isinya key dan value konfigurasi. Banyak setting yang di Linux mungkin tersebar di file text, di Windows sering tersimpan di registry. Registry dipakai oleh OS, driver, service, shell, aplikasi, dan policy.

Registry bukan satu file tunggal. Beberapa hive dimuat dari file berbeda saat boot atau saat user login. Karena itu setting machine-wide dan setting user dipisah.

Hive penting:

| Hive | Arti |
|---|---|
| HKEY_LOCAL_MACHINE | konfigurasi machine-wide |
| HKEY_CURRENT_USER | konfigurasi user yang sedang login |
| HKEY_CLASSES_ROOT | file association dan COM registration |
| HKEY_USERS | semua user hive yang sedang dimuat |
| HKEY_CURRENT_CONFIG | konfigurasi hardware aktif |

Hive file umum:

| Hive | File Umum |
|---|---|
| HKLM\SYSTEM | `C:\Windows\System32\Config\SYSTEM` |
| HKLM\SOFTWARE | `C:\Windows\System32\Config\SOFTWARE` |
| HKLM\SAM | `C:\Windows\System32\Config\SAM` |
| HKLM\SECURITY | `C:\Windows\System32\Config\SECURITY` |
| HKCU | `C:\Users\<user>\NTUSER.DAT` |

Contoh:

```text
HKLM\SYSTEM\CurrentControlSet\Services
```

Path ini menyimpan konfigurasi banyak service dan driver. Jika driver boot-start salah konfigurasi, sistem bisa gagal boot.

Singkatan PowerShell:

| PSDrive | Registry Hive |
|---|---|
| `HKLM:` | HKEY_LOCAL_MACHINE |
| `HKCU:` | HKEY_CURRENT_USER |

Registry value type:

| Type | Arti |
|---|---|
| REG_SZ | string |
| REG_DWORD | angka 32-bit |
| REG_QWORD | angka 64-bit |
| REG_MULTI_SZ | multi-string |
| REG_EXPAND_SZ | string dengan environment variable |
| REG_BINARY | data binary |

Command registry:

```powershell
# Melihat key registry.
Get-ChildItem HKLM:\SOFTWARE

# Membaca value registry.
Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion

# Membuat backup registry key ke file .reg.
reg export HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion currentversion.reg
```

Catatan:

- Backup registry sebelum perubahan manual.
- Banyak policy domain menulis registry di bawah area `Policies`.
- Salah registry bisa merusak aplikasi, profile, service, atau boot.
- Jangan menganggap semua registry change langsung aktif; beberapa butuh service restart, user logoff, atau reboot.
- Untuk perubahan enterprise, lebih baik gunakan policy atau management tool daripada edit manual satu per satu.

### 1.5 Processes, Services, dan Scheduled Tasks

Process adalah program yang sedang berjalan. Service adalah process/background component yang dikelola Service Control Manager.

Process bisa dibuat oleh user, service, scheduled task, aplikasi lain, atau sistem. Setiap process punya PID, memory, handle, thread, dan token security. Token menentukan process itu berjalan sebagai siapa dan punya privilege apa.

Service berbeda dari aplikasi biasa karena lifecycle-nya dikelola Windows. Service bisa start saat boot, berjalan tanpa user login, memakai account khusus, dan punya dependency. Scheduled task berbeda lagi: task berjalan berdasarkan trigger seperti jam tertentu, logon user, event, idle, atau startup.

Perbedaan praktis:

| Item | Kapan Berjalan | Context | Contoh |
|---|---|---|---|
| Process user | saat user/app menjalankan | token user | browser, Notepad |
| Service | saat SCM menjalankan | LocalSystem/LocalService/user service | Spooler, WinRM |
| Scheduled task | saat trigger terpenuhi | principal task | backup script, cleanup |

Process field:

| Field | Arti |
|---|---|
| PID | process ID |
| PPID | parent process ID |
| CPU | penggunaan CPU |
| Working set | memory fisik yang sedang dipakai |
| Handle | referensi ke object kernel |
| Token | security context process |

Service startup type:

| Startup Type | Arti |
|---|---|
| Automatic | start saat boot |
| Automatic Delayed Start | start setelah service utama |
| Manual | start saat dibutuhkan |
| Disabled | tidak bisa start sampai diubah |

Scheduled task:

| Komponen | Arti |
|---|---|
| Trigger | kapan task berjalan |
| Action | program/script yang dijalankan |
| Principal | user/security context |
| Condition | syarat tambahan |
| History | riwayat eksekusi |

Service dependency:

- Beberapa service membutuhkan service lain.
- Jika dependency gagal start, service utama bisa gagal.
- Error service sering muncul di System log.
- Service network bisa terlihat running tetapi aplikasinya tetap gagal jika port tidak listen.

Troubleshooting cepat:

| Gejala | Cek |
|---|---|
| App hang | process CPU/memory, event Application |
| Service tidak start | dependency, account, permission, System log |
| Task tidak jalan | trigger, principal, last run result, history |
| CPU tinggi | process tree, thread, antivirus scan, update |
| Memory tinggi | working set, commit, leak, handle count |

Command process/service/task:

```powershell
# Melihat process yang sedang berjalan.
Get-Process

# Melihat process tertentu.
Get-Process -Name explorer

# Melihat service.
Get-Service

# Melihat service yang sedang running.
Get-Service | Where-Object Status -eq Running

# Restart service tertentu.
Restart-Service -Name Spooler

# Melihat scheduled task.
Get-ScheduledTask

# Melihat scheduled task yang aktif.
Get-ScheduledTask | Where-Object State -eq Ready
```

---

## 2.0 System Administration

### 2.1 Local Users and Groups

Local user disimpan di komputer lokal. Ini berbeda dari domain user yang dikelola Active Directory.

Local user hanya valid di komputer tempat account itu dibuat. Jika kamu membuat user `labuser` di PC-A, user itu tidak otomatis ada di PC-B. Windows membedakan identity menggunakan SID, bukan hanya nama. Dua komputer bisa sama-sama punya user bernama `labuser`, tetapi SID-nya berbeda, sehingga Windows menganggap itu dua identity berbeda.

Local group dipakai untuk memberi hak ke kumpulan user. Best practice-nya: beri permission ke group, bukan langsung ke user satu per satu. Dengan begitu saat orang masuk/keluar role, kamu cukup mengubah membership group.

Local group penting:

| Group | Hak Umum |
|---|---|
| Administrators | kontrol administratif lokal |
| Users | user biasa |
| Remote Desktop Users | boleh login via RDP |
| Event Log Readers | boleh membaca event log |
| Backup Operators | hak backup/restore tertentu |
| Power Users | legacy, hindari untuk desain modern |

User account field:

| Field | Arti |
|---|---|
| Name | username |
| SID | security identifier |
| Enabled | status aktif/nonaktif |
| LastLogon | login terakhir |
| PasswordRequired | apakah butuh password |
| PasswordLastSet | kapan password terakhir diubah |

SID:

```text
S-1-5-21-1111111111-2222222222-3333333333-1001
```

Field umum SID:

| Bagian | Arti |
|---|---|
| `S` | SID string |
| `1` | revision |
| `5` | identifier authority |
| `21-...` | machine/domain identifier |
| `1001` | RID, identifier object dalam machine/domain itu |

Built-in RID yang sering muncul:

| RID | Arti |
|---:|---|
| 500 | built-in Administrator |
| 501 | built-in Guest |
| 544 | Administrators group |
| 545 | Users group |

Kenapa SID penting:

- Permission sebenarnya menempel ke SID.
- Rename user tidak mengubah SID.
- User yang dihapus lalu dibuat ulang dengan nama sama tetap SID baru.
- ACL bisa menampilkan SID mentah jika account asalnya sudah hilang.

Command user/group:

```powershell
# Melihat local user.
Get-LocalUser

# Melihat local group.
Get-LocalGroup

# Melihat member group Administrators.
Get-LocalGroupMember Administrators

# Membuat local user baru.
New-LocalUser -Name labuser -NoPassword

# Menambahkan user ke group lokal.
Add-LocalGroupMember -Group "Remote Desktop Users" -Member labuser

# Disable local user.
Disable-LocalUser -Name labuser
```

### 2.2 NTFS Permission, Share Permission, dan UAC

Windows memiliki beberapa layer akses.

Permission Windows sering terasa membingungkan karena akses file lewat network melewati dua gate: share permission dan NTFS permission. Jika salah satu menolak, akses gagal. Selain itu, user administrator belum tentu sedang elevated karena UAC membuat token admin terfilter sampai user memilih "Run as administrator".

Contoh sederhana:

```text
User Alice akses \\fileserver\Data\report.xlsx

1. Alice harus boleh lewat share permission Data.
2. Alice harus boleh lewat NTFS permission folder/file.
3. Jika aplikasi butuh admin, token Alice harus elevated.
```

| Layer | Fungsi |
|---|---|
| Share permission | berlaku saat akses lewat network share |
| NTFS permission | berlaku lokal dan network |
| UAC | membatasi elevation administrator |
| Ownership | siapa pemilik object |
| Inheritance | permission turun dari parent folder |

NTFS permission umum:

| Permission | Arti |
|---|---|
| Full Control | semua hak termasuk permission change |
| Modify | read/write/delete |
| Read & Execute | baca dan jalankan |
| List Folder Contents | melihat isi folder |
| Read | baca |
| Write | tulis |

Effective permission:

```text
Effective access = kombinasi allow/deny + group membership + inheritance + ownership + elevation context
```

Contoh effective permission:

| Share Permission | NTFS Permission | Hasil |
|---|---|---|
| Read | Modify | Read |
| Full Control | Read | Read |
| Change | Modify | Modify/Change sesuai kombinasi |
| Deny | Full Control | Denied |

Inheritance:

- Folder parent bisa menurunkan permission ke child folder/file.
- Child bisa mewarisi permission atau memutus inheritance.
- Permission explicit biasanya lebih mudah diaudit daripada banyak exception acak.
- Terlalu banyak explicit permission membuat troubleshooting sulit.

Aturan penting:

- Explicit deny biasanya menang dari allow.
- NTFS permission tetap berlaku walau share permission longgar.
- Best practice umum: share permission dibuat luas, NTFS permission dibuat spesifik.
- UAC membuat administrator tidak selalu berjalan dengan token elevated.

UAC:

| Kondisi | Token |
|---|---|
| User biasa | standard user token |
| Admin login normal | filtered admin token |
| Run as administrator | elevated admin token |
| Service LocalSystem | system token |

Gejala UAC umum:

| Gejala | Kemungkinan |
|---|---|
| Command gagal access denied di terminal biasa | belum elevated |
| Bisa buka folder tapi gagal ubah file sistem | butuh elevated token |
| Script jalan manual tapi gagal dari task | principal task berbeda |
| Admin tidak bisa akses share tertentu | share/NTFS tidak memberi hak ke SID/group yang benar |

Command permission:

```powershell
# Melihat ACL folder.
Get-Acl C:\Data

# Melihat ACL dengan tool icacls.
icacls C:\Data

# Memberi permission read/execute dengan icacls.
icacls C:\Data /grant "Users:(RX)"

# Melihat share SMB.
Get-SmbShare

# Melihat permission share SMB.
Get-SmbShareAccess -Name ShareName
```

### 2.3 Storage, Disk, Volume, dan BitLocker

Windows storage punya beberapa layer.

Saat troubleshooting storage, penting membedakan disk fisik/virtual, partition, volume, filesystem, dan drive letter. User biasanya hanya melihat `C:` atau `D:`, tetapi admin perlu tahu layer di bawahnya. Misalnya masalah "drive D hilang" bisa disebabkan disk offline, partition hilang, volume tidak punya drive letter, filesystem corrupt, atau BitLocker locked.

```text
Disk
-> partition
-> volume
-> filesystem
-> mount point / drive letter
```

Konsep:

| Item | Arti |
|---|---|
| Disk | perangkat storage fisik/virtual |
| Partition | pembagian disk |
| Volume | area storage yang diformat |
| Drive letter | huruf seperti `C:` |
| Mount point | volume dipasang ke folder |
| NTFS | filesystem utama Windows |
| ReFS | filesystem untuk workload server tertentu |
| Storage Spaces | pooling disk Windows |

Contoh mapping:

| Layer | Contoh |
|---|---|
| Disk | `Disk 0` SSD 512 GB |
| Partition | EFI, MSR, Windows, Recovery |
| Volume | `C:` NTFS |
| Filesystem | NTFS |
| Mount | drive letter `C:` |

Partition style:

| Style | Catatan |
|---|---|
| MBR | legacy, limit lama |
| GPT | modern, umum untuk UEFI |

BitLocker:

| Item | Arti |
|---|---|
| TPM | chip untuk menyimpan secret secara aman |
| Recovery key | key pemulihan saat unlock normal gagal |
| Used space only | enkripsi area yang sudah dipakai |
| Full disk | enkripsi seluruh volume |

BitLocker flow:

```text
Boot
-> TPM/protector validasi kondisi boot
-> volume key dibuka
-> Windows membaca volume terenkripsi
```

Recovery key bisa diminta jika:

- TPM berubah atau reset.
- boot configuration berubah.
- firmware/secure boot berubah.
- disk dipindah ke komputer lain.
- protector rusak atau policy berubah.

Storage troubleshooting:

| Gejala | Dugaan |
|---|---|
| Disk tidak muncul | driver/controller/virtual disk/power |
| Disk offline | policy SAN/offline/manual state |
| Volume tidak punya letter | mount point/drive letter belum ada |
| Access denied | NTFS permission/BitLocker |
| File corrupt | filesystem/storage issue |
| Disk latency tinggi | storage bottleneck, queue, failing disk |

Command storage:

```powershell
# Melihat disk.
Get-Disk

# Melihat partition.
Get-Partition

# Melihat volume.
Get-Volume

# Melihat BitLocker volume.
Get-BitLockerVolume

# Melihat physical disk untuk Storage Spaces.
Get-PhysicalDisk
```

### 2.4 Apps, Packages, dan Windows Update

Windows punya beberapa jalur instalasi aplikasi.

Windows tidak punya satu package manager universal seperti banyak distro Linux. Aplikasi bisa datang dari MSI, EXE installer, Microsoft Store, MSIX/Appx, winget, Intune, SCCM, atau script deployment. Karena itu inventory aplikasi dan uninstall aplikasi bisa berbeda-beda tergantung cara instalasinya.

| Tipe | Contoh |
|---|---|
| MSI | installer enterprise klasik |
| EXE installer | installer vendor |
| MSIX/Appx | modern app package |
| Microsoft Store | store app |
| Winget | package manager Windows |

Windows Update:

| Item | Arti |
|---|---|
| Quality update | security/bugfix bulanan |
| Feature update | upgrade versi/build Windows |
| Driver update | update driver |
| WSUS | update server internal |
| Delivery Optimization | distribusi update peer/cache |

MSI vs EXE vs MSIX:

| Tipe | Ciri |
|---|---|
| MSI | installer database dengan product code, cocok enterprise deployment |
| EXE | wrapper vendor, perilaku tergantung vendor |
| MSIX/Appx | package modern dengan manifest |
| Portable app | tidak selalu terdaftar sebagai installed app |

Windows Update lifecycle:

```text
Scan
-> Download
-> Install/stage
-> Pending reboot jika perlu
-> Commit setelah reboot
-> Cleanup/supersedence
```

Masalah update umum:

| Gejala | Dugaan |
|---|---|
| Update stuck download | network/proxy/cache/Windows Update service |
| Install gagal | component store, disk space, servicing stack |
| Reboot loop | pending operation/driver/update failure |
| App rusak setelah update | compatibility/regression |
| Driver berubah sendiri | driver update policy |

Command app/update:

```powershell
# Melihat aplikasi MSI yang terdaftar.
Get-CimInstance Win32_Product

# Melihat Appx package user saat ini.
Get-AppxPackage

# Mencari package dengan winget.
winget search vscode

# Melihat update history.
Get-HotFix
```

Catatan:

- `Win32_Product` bisa lambat dan memicu repair MSI pada beberapa sistem; pakai hati-hati.
- Untuk inventory enterprise, tool management seperti Intune, SCCM, atau inventory agent lebih cocok.
- Untuk troubleshooting update, lihat juga CBS log, DISM log, Windows Update log, dan Event Viewer.
- Jangan hapus folder sistem update secara manual tanpa prosedur yang jelas.

### 2.5 Devices and Drivers

Driver berjalan dekat kernel dan bisa menyebabkan crash, boot issue, atau performance issue.

Driver adalah software yang membuat Windows bisa berbicara dengan hardware atau device virtual. Ada driver user-mode dan kernel-mode, tetapi driver yang berjalan di kernel mode punya dampak paling besar jika rusak. Network adapter, storage controller, GPU, antivirus filter driver, dan VPN client driver sering muncul dalam troubleshooting performa atau blue screen.

Plug and Play membantu Windows mendeteksi device, mencocokkan hardware ID dengan driver, lalu membuat device node. Kalau driver tidak cocok, device bisa muncul sebagai unknown device atau error code di Device Manager.

Device Manager category:

| Category | Contoh |
|---|---|
| Network adapters | NIC, Wi-Fi |
| Disk drives | SSD/HDD/virtual disk |
| Display adapters | GPU |
| Human Interface Devices | keyboard/mouse |
| System devices | chipset, ACPI, bus |

Driver state:

| State | Arti |
|---|---|
| Working | device normal |
| Disabled | device dimatikan |
| Error code | driver/device bermasalah |
| Unknown device | driver belum cocok |

Driver package:

| Bagian | Arti |
|---|---|
| INF | metadata instalasi driver |
| SYS | driver binary, sering kernel-mode |
| CAT | catalog signature |
| Driver Store | repository driver Windows |
| Hardware ID | identifier device untuk matching driver |

Error code umum:

| Code | Arti Ringkas |
|---:|---|
| 10 | device cannot start |
| 28 | driver not installed |
| 31 | driver gagal load/bermasalah |
| 43 | device melaporkan problem ke Windows |

Troubleshooting driver:

| Gejala | Cek |
|---|---|
| Device hilang | Device Manager, PnP status, BIOS/UEFI |
| BSOD setelah driver update | minidump, rollback driver |
| Network putus setelah VPN install | filter driver, adapter binding |
| Disk lambat | storage driver, firmware, queue, event log |

Command device/driver:

```powershell
# Melihat PnP device.
Get-PnpDevice

# Melihat device bermasalah.
Get-PnpDevice | Where-Object Status -ne OK

# Melihat driver signed.
Get-WindowsDriver -Online

# Melihat driver dengan pnputil.
pnputil /enum-drivers
```

---

## 3.0 Windows Networking

### 3.1 TCP/IP Configuration

Windows networking memakai interface, IP address, route, DNS client, firewall profile, dan adapter binding.

Pada Windows, koneksi network tidak hanya "ada IP". Satu adapter punya status link, MAC address, IP address, prefix length, default gateway, DNS server, firewall profile, metric, dan binding protocol. Kalau salah satu bagian salah, hasilnya bisa berbeda: link up tapi tidak dapat IP, IP ada tapi tidak bisa keluar subnet, DNS gagal, atau port tertentu diblok firewall.

Urutan berpikir:

```text
Adapter up?
-> IP address benar?
-> Prefix/mask benar?
-> Default gateway benar?
-> DNS benar?
-> Route benar?
-> Firewall/app port benar?
```

Interface field:

| Field | Arti |
|---|---|
| InterfaceAlias | nama adapter |
| InterfaceIndex | ID interface |
| MACAddress | alamat Layer 2 |
| LinkSpeed | speed link |
| Status | Up/Down |

DHCP vs static:

| Mode | Arti | Kapan Dipakai |
|---|---|---|
| DHCP | IP diberikan otomatis oleh DHCP server | client endpoint umum |
| Static | IP ditulis manual | server, printer, network appliance |
| APIPA | `169.254.x.x` saat DHCP gagal | tanda host tidak dapat lease |

Route dan metric:

- Windows memilih route paling spesifik lebih dulu.
- Jika ada beberapa route cocok, metric membantu memilih route.
- VPN sering menambahkan route baru.
- Default route `0.0.0.0/0` dipakai untuk destination yang tidak punya route lebih spesifik.

Contoh masalah:

| Gejala | Kemungkinan |
|---|---|
| IP `169.254.x.x` | DHCP gagal |
| Bisa ping gateway, tidak bisa internet | route/NAT/firewall/upstream |
| Bisa akses IP, nama gagal | DNS |
| Bisa browsing, RDP gagal | firewall/service/port |
| Setelah VPN connect internet mati | full tunnel/default route/proxy |

Command IP:

```powershell
# Melihat konfigurasi IP lengkap.
ipconfig /all

# Melihat adapter network.
Get-NetAdapter

# Melihat IP address.
Get-NetIPAddress

# Melihat route table.
Get-NetRoute

# Melihat default route IPv4.
Get-NetRoute -DestinationPrefix 0.0.0.0/0

# Melihat neighbor table ARP/ND.
Get-NetNeighbor

# Menguji koneksi TCP ke port 443.
Test-NetConnection example.com -Port 443
```

IP static sementara:

```powershell
# Menambahkan IPv4 address ke interface.
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.50.10 -PrefixLength 24 -DefaultGateway 192.168.50.1

# Mengatur DNS server pada interface.
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 1.1.1.1,8.8.8.8
```

### 3.2 DNS Client, Hosts File, dan Name Resolution

Windows name resolution bisa melibatkan hosts file, DNS cache, DNS server, LLMNR/NetBIOS pada environment tertentu.

Name resolution adalah proses mengubah nama seperti `server01` atau `app.example.com` menjadi IP address. Windows bisa mencoba beberapa mekanisme tergantung konfigurasi. Untuk environment modern, DNS adalah yang utama. Hosts file dan DNS cache sering membuat troubleshooting membingungkan karena jawaban bisa datang dari cache/lokal, bukan dari DNS server yang kamu kira.

Hosts file:

```text
C:\Windows\System32\drivers\etc\hosts
```

Alur sederhana:

```text
Application minta resolve nama
-> cek hosts file/cache/local resolver behavior
-> DNS client bertanya ke DNS server
-> jawaban disimpan di cache sesuai TTL
-> aplikasi memakai IP hasil resolve
```

Jenis nama:

| Nama | Contoh | Catatan |
|---|---|---|
| Hostname pendek | `server01` | bisa bergantung DNS suffix/search |
| FQDN | `server01.example.com` | nama lengkap |
| NetBIOS name | `SERVER01` | legacy Windows |
| mDNS/LLMNR | local name resolution | bisa dimatikan untuk hardening |

DNS suffix:

- DNS suffix membantu Windows mengubah nama pendek menjadi FQDN.
- Misalnya `server01` bisa dicoba sebagai `server01.corp.example.com`.
- Salah DNS suffix bisa membuat host mencari nama di domain yang salah.

DNS troubleshooting:

```powershell
# Query DNS sederhana.
nslookup example.com

# Resolve DNS dengan PowerShell.
Resolve-DnsName example.com

# Melihat DNS cache client.
Get-DnsClientCache

# Membersihkan DNS cache.
Clear-DnsClientCache

# Melihat DNS server pada interface.
Get-DnsClientServerAddress
```

Gejala DNS:

| Gejala | Dugaan |
|---|---|
| IP bisa, nama gagal | DNS client/server/record |
| Nama resolve ke IP lama | cache/TTL/hosts file |
| Domain resource gagal | DNS suffix/search/domain DNS |
| Hanya satu aplikasi gagal | proxy/app config/DoH internal |

Cache:

- Cache mempercepat lookup.
- Cache juga bisa membuat jawaban lama bertahan sampai TTL habis.
- `Clear-DnsClientCache` hanya membersihkan cache client lokal, bukan cache DNS server.

### 3.3 Windows Firewall and Remote Access

Windows Firewall memakai profile.

Windows Firewall adalah host-based firewall. Artinya rule diterapkan di komputer itu sendiri, bukan hanya di firewall network. Ini penting karena traffic bisa sudah sampai ke host tetapi tetap diblok oleh firewall lokal. Profile firewall berubah tergantung network location: domain, private, atau public.

Remote management seperti RDP dan PowerShell Remoting butuh tiga hal sekaligus: service/listener aktif, firewall mengizinkan, dan authentication/authorization benar.

| Profile | Arti |
|---|---|
| Domain | saat host mengenali domain network |
| Private | trusted private network |
| Public | untrusted network |

Remote access:

| Teknologi | Fungsi |
|---|---|
| RDP | remote desktop GUI |
| PowerShell Remoting | remote command/admin |
| WinRM | transport remoting |
| SMB admin share | file/admin access |

Remote access dependency:

| Teknologi | Service/Port Umum | Catatan |
|---|---|---|
| RDP | TCP/UDP 3389 | butuh Remote Desktop enabled dan user punya hak |
| WinRM HTTP | TCP 5985 | PowerShell Remoting default internal |
| WinRM HTTPS | TCP 5986 | memakai certificate |
| SMB | TCP 445 | file share/admin share |

Profile behavior:

| Profile | Default Sikap |
|---|---|
| Public | paling ketat, cocok jaringan tidak dipercaya |
| Private | lebih longgar untuk LAN terpercaya |
| Domain | dipakai saat host mengenali domain environment |

Gejala umum:

| Gejala | Kemungkinan |
|---|---|
| RDP timeout | firewall/network/service tidak listen |
| RDP credential rejected | user/password/policy/NLA |
| WinRM access denied | permission/auth/trusted hosts/UAC remote |
| Bisa ping tapi remote gagal | firewall port atau service remote |

Firewall command:

```powershell
# Melihat firewall profile.
Get-NetFirewallProfile

# Melihat firewall rules yang enabled.
Get-NetFirewallRule -Enabled True

# Mencari rule berdasarkan display name.
Get-NetFirewallRule -DisplayName "*Remote Desktop*"

# Menguji apakah WinRM aktif.
Test-WSMan localhost
```

### 3.4 SMB and File Sharing

SMB dipakai untuk file sharing, admin share, printer sharing, dan banyak operasi Windows enterprise.

SMB adalah protocol utama Windows untuk akses file lewat network. Saat user membuka `\\server\share`, client membuat koneksi SMB ke server. Akses akhir dipengaruhi oleh share permission, NTFS permission, authentication, firewall, SMB version, signing/encryption, dan kadang name resolution.

Path UNC:

```text
\\server\share\folder
```

Share umum:

| Share | Arti |
|---|---|
| custom share | share buatan admin |
| `C$` | administrative share drive C |
| `ADMIN$` | admin share Windows directory |
| `IPC$` | inter-process communication |

SMB security:

| Fitur | Arti |
|---|---|
| SMB signing | integritas traffic SMB |
| SMB encryption | enkripsi traffic SMB |
| NTFS permission | permission filesystem |
| Share permission | permission share network |

Alur akses SMB:

```text
Client resolve nama server
-> koneksi TCP 445
-> SMB negotiate
-> authentication
-> tree connect ke share
-> cek share permission
-> cek NTFS permission
-> file open/read/write
```

Share vs NTFS:

| Layer | Berlaku Saat | Contoh |
|---|---|---|
| Share permission | akses lewat `\\server\share` | user boleh masuk share |
| NTFS permission | akses lokal dan network | user boleh baca/tulis file |

SMB troubleshooting:

| Gejala | Dugaan |
|---|---|
| `\\server` tidak resolve | DNS/NetBIOS/name issue |
| Connection timeout | firewall TCP 445/network |
| Access denied | share/NTFS permission/auth |
| File terkunci | open handle/session |
| Lambat | network, antivirus, oplock/cache, server disk |

Command SMB:

```powershell
# Melihat SMB share lokal.
Get-SmbShare

# Membuat SMB share.
New-SmbShare -Name Data -Path C:\Data -FullAccess Administrators -ReadAccess Users

# Melihat session SMB aktif.
Get-SmbSession

# Melihat file SMB yang sedang terbuka.
Get-SmbOpenFile
```

---

## 4.0 Windows Security

### 4.1 Authentication, Tokens, dan Credential

Windows security banyak bergantung pada SID, token, privilege, ACL, dan credential.

Saat user login, Windows tidak hanya menyimpan "nama user". Windows membuat logon session dan access token. Token ini berisi SID user, SID group, privilege, integrity level, dan informasi lain yang dipakai saat process mengakses object. Setiap kali process membuka file, registry key, service, atau resource lain, Windows membandingkan token process dengan security descriptor object.

Ini alasan kenapa dua user dengan nama sama belum tentu sama, dan kenapa administrator kadang tetap mendapat "Access denied" jika belum elevated.

Konsep:

| Item | Arti |
|---|---|
| SID | security identifier untuk user/group/computer |
| Access token | security context process/thread |
| Privilege | hak khusus seperti shutdown/debug/backup |
| ACL | daftar allow/deny pada object |
| LSA | Local Security Authority |
| LSASS | process yang menangani security/authentication |
| SAM | local account database |
| Credential Manager | penyimpanan credential user |

Token berisi:

| Isi Token | Arti |
|---|---|
| User SID | identity utama user |
| Group SIDs | group membership |
| Privileges | hak khusus seperti backup/debug |
| Integrity level | low/medium/high/system |
| Logon session | sesi authentication |
| Elevation state | elevated atau filtered |

Local vs domain auth:

| Item | Local | Domain |
|---|---|---|
| Account storage | SAM lokal | Active Directory |
| Scope | satu komputer | domain |
| Protocol | local logon, NTLM dapat muncul | Kerberos/NTLM |
| Policy | local policy | Group Policy |

Access check sederhana:

```text
Process membawa token
-> process mencoba membuka object
-> Windows membaca security descriptor object
-> DACL/deny/allow/integrity/privilege dievaluasi
-> access granted atau access denied
```

Credential:

- Credential bisa berupa password, hash, ticket, certificate, key, atau token.
- Credential Manager menyimpan credential tertentu untuk user.
- LSASS memegang bagian penting dari authentication state.
- Karena itu proteksi LSASS dan disk encryption penting untuk endpoint security.

Command identity/security:

```powershell
# Melihat identity user saat ini.
whoami

# Melihat group dan privilege token user.
whoami /all

# Melihat SID user.
whoami /user

# Melihat credential manager lewat GUI command.
control keymgr.dll
```

### 4.2 Defender, Firewall, dan Attack Surface Reduction

Windows Defender dan security feature modern membantu endpoint protection.

Endpoint security bukan hanya antivirus. Windows modern punya beberapa lapisan: antivirus, firewall, SmartScreen, attack surface reduction, exploit protection, controlled folder access, BitLocker, dan logging. Tujuannya bukan membuat host "tidak mungkin diserang", tetapi mengurangi peluang eksekusi malware, membatasi lateral movement, dan memberi bukti saat incident.

| Fitur | Fungsi |
|---|---|
| Microsoft Defender Antivirus | anti-malware |
| Real-time protection | scanning aktif |
| Controlled folder access | proteksi folder dari ransomware |
| Attack Surface Reduction | aturan pengurangan teknik serangan |
| SmartScreen | reputasi file/site |
| Windows Firewall | host firewall |
| BitLocker | disk encryption |

Peran tiap layer:

| Layer | Melindungi Dari |
|---|---|
| Defender Antivirus | malware known/suspicious |
| ASR rules | pola serangan umum seperti Office child process |
| Controlled folder access | ransomware menulis folder penting |
| Firewall | inbound/outbound traffic yang tidak diizinkan |
| SmartScreen | file/site reputasi buruk |
| BitLocker | pencurian data saat disk offline/dicuri |

Operational note:

- Security feature bisa memblokir aplikasi legitimate jika rule terlalu agresif.
- Selalu cek event/log sebelum disable proteksi.
- Untuk enterprise, perubahan sebaiknya lewat policy/management tool agar konsisten.

Command Defender:

```powershell
# Melihat status Defender.
Get-MpComputerStatus

# Melihat preference Defender.
Get-MpPreference

# Update signature Defender.
Update-MpSignature

# Scan cepat.
Start-MpScan -ScanType QuickScan
```

### 4.3 Logging, Auditing, dan Event Viewer

Event log adalah sumber bukti utama Windows.

Windows mencatat banyak kejadian sebagai event. Event log bukan hanya untuk error; ia juga dipakai untuk audit login, perubahan service, update, PowerShell activity, Defender, driver, dan crash. Saat troubleshooting, event log memberi timeline: apa yang terjadi duluan, apa yang berubah, dan error apa yang muncul.

Kunci membaca event:

- `LogName`: log mana yang menyimpan event.
- `Provider`: komponen yang menulis event.
- `Event ID`: jenis event.
- `Level`: critical, error, warning, information.
- `TimeCreated`: kapan terjadi.
- `User/Computer`: konteks kejadian.

Log penting:

| Log | Isi |
|---|---|
| System | driver, service, boot, hardware |
| Application | aplikasi |
| Security | logon, audit, privilege, policy |
| Setup | instalasi/update |
| PowerShell | PowerShell operational log |
| Windows Defender | antivirus/security events |

Event ID umum:

| Event ID | Log | Arti |
|---:|---|---|
| 4624 | Security | successful logon |
| 4625 | Security | failed logon |
| 4634 | Security | logoff |
| 4672 | Security | special privileges assigned |
| 4688 | Security | process creation jika audit aktif |
| 7045 | System | service installed |
| 6005 | System | event log service started |
| 6006 | System | event log service stopped |
| 6008 | System | unexpected shutdown |

Level event:

| Level | Arti |
|---|---|
| Critical | masalah serius |
| Error | operasi gagal |
| Warning | kondisi perlu perhatian |
| Information | event normal/informatif |
| Verbose/Debug | detail tambahan jika aktif |

Audit policy:

- Security log tidak otomatis mencatat semua hal.
- Beberapa event seperti process creation butuh audit policy aktif.
- Terlalu banyak audit bisa membuat log cepat penuh.
- Terlalu sedikit audit membuat incident sulit direkonstruksi.

Command event:

```powershell
# Melihat daftar log.
Get-WinEvent -ListLog *

# Melihat event System terbaru.
Get-WinEvent -LogName System -MaxEvents 20

# Melihat failed logon terbaru.
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 10

# Melihat service installed event.
Get-WinEvent -FilterHashtable @{LogName='System'; Id=7045} -MaxEvents 10
```

### 4.4 Hardening Baseline

Hardening mengurangi attack surface tanpa merusak operasional.

Hardening adalah proses membuat konfigurasi lebih aman tanpa membuat sistem tidak bisa dipakai. Ini bukan satu command, melainkan baseline: account, service, firewall, update, logging, encryption, remote access, dan application control. Hardening yang baik harus bisa dijelaskan, diuji, dan di-rollback.

Checklist hardening:

| Area | Praktik |
|---|---|
| Account | disable unused account, least privilege |
| Password | enforce policy/MFA jika tersedia |
| Services | disable service yang tidak dipakai |
| Firewall | default deny inbound yang tidak perlu |
| Updates | patch rutin |
| Defender | real-time protection aktif |
| BitLocker | encrypt endpoint |
| Logging | audit logon/process/security event |
| Remote access | batasi RDP/WinRM ke admin network |

Prinsip hardening:

| Prinsip | Arti |
|---|---|
| Least privilege | user/service hanya punya hak yang dibutuhkan |
| Reduce attack surface | matikan fitur/service yang tidak perlu |
| Secure defaults | konfigurasi default dibuat aman |
| Auditability | aksi penting tercatat |
| Recoverability | ada backup/recovery plan |

Contoh hardening yang berisiko jika asal:

| Perubahan | Risiko |
|---|---|
| Disable service massal | aplikasi/OS feature bisa rusak |
| Blok outbound firewall semua | update/activation/app bisa gagal |
| ASR terlalu ketat | macro/app bisnis bisa terganggu |
| Audit terlalu verbose | log cepat penuh dan noise tinggi |

Command baseline:

```powershell
# Melihat local security policy export.
secedit /export /cfg C:\Temp\secpol.cfg

# Melihat status BitLocker.
Get-BitLockerVolume

# Melihat local administrators.
Get-LocalGroupMember Administrators

# Melihat inbound firewall rule yang enabled.
Get-NetFirewallRule -Direction Inbound -Enabled True
```

---

## 5.0 PowerShell

### 5.1 Cmdlet, Object, Pipeline, dan Help

PowerShell mengirim object, bukan hanya text.

Ini perbedaan besar dari shell tradisional. Output `Get-Process` bukan sekadar text tabel; ia adalah object dengan property seperti `Name`, `Id`, `CPU`, `WorkingSet`, dan method tertentu. Karena itu output bisa difilter, diurutkan, dipilih field-nya, lalu diekspor tanpa parsing text manual.

Pola cmdlet:

```text
Verb-Noun
```

Contoh:

| Cmdlet | Fungsi |
|---|---|
| `Get-Process` | mengambil process |
| `Get-Service` | mengambil service |
| `Get-ChildItem` | list file/registry/provider item |
| `Where-Object` | filter object |
| `Select-Object` | memilih property |
| `Sort-Object` | sorting |
| `Export-Csv` | export object ke CSV |

Pipeline object:

```text
Get-Process
-> mengirim object process
Where-Object CPU -gt 100
-> menyaring object
Select-Object Name, Id, CPU
-> memilih property
Export-Csv
-> menyimpan structured data
```

Verb umum:

| Verb | Arti |
|---|---|
| Get | mengambil data |
| Set | mengubah konfigurasi |
| New | membuat object/resource |
| Remove | menghapus |
| Start | menjalankan |
| Stop | menghentikan |
| Restart | restart |
| Test | menguji kondisi |

Tips belajar:

- Gunakan `Get-Help` sebelum menjalankan command yang mengubah sistem.
- Gunakan `-WhatIf` jika tersedia untuk simulasi.
- Gunakan `Select-Object` untuk melihat field penting.
- Gunakan `Get-Member` untuk memahami object.

Command dasar:

```powershell
# Mencari command berdasarkan noun.
Get-Command -Noun Service

# Membaca help command.
Get-Help Get-Service

# Membaca contoh penggunaan command.
Get-Help Get-Service -Examples

# Melihat property object process.
Get-Process | Get-Member

# Filter service running.
Get-Service | Where-Object Status -eq Running

# Pilih property tertentu.
Get-Process | Select-Object Name, Id, CPU
```

### 5.2 Modules, Execution Policy, dan Profiles

Module menyimpan cmdlet/function/provider.

PowerShell diperluas lewat module. Module bisa berasal dari Windows bawaan, role/feature tertentu, aplikasi, atau PowerShell Gallery. Kalau sebuah command tidak ditemukan, bisa jadi module belum terpasang, belum diimport, atau kamu memakai PowerShell versi yang berbeda.

| Item | Arti |
|---|---|
| Module | paket command |
| Gallery | repository module |
| Execution Policy | guardrail script execution |
| Profile | script startup PowerShell user |

Execution policy bukan security boundary penuh. Ini lebih seperti safety control agar script tidak mudah berjalan tanpa sengaja.

Execution policy:

| Policy | Arti Ringkas |
|---|---|
| Restricted | script tidak boleh berjalan |
| RemoteSigned | script lokal boleh, script internet perlu signed |
| AllSigned | semua script perlu signed |
| Bypass | tidak membatasi script |
| Undefined | tidak dikonfigurasi pada scope itu |

Scope:

| Scope | Prioritas |
|---|---|
| MachinePolicy | tertinggi, biasanya GPO |
| UserPolicy | dari policy user |
| Process | hanya process PowerShell saat ini |
| CurrentUser | user saat ini |
| LocalMachine | seluruh komputer |

Profile:

- Profile adalah script yang jalan saat PowerShell start.
- Berguna untuk alias/function pribadi.
- Bisa juga menjadi sumber masalah jika profile memuat script lambat atau error.

Command module:

```powershell
# Melihat module yang tersedia.
Get-Module -ListAvailable

# Melihat module yang sedang loaded.
Get-Module

# Import module.
Import-Module Microsoft.PowerShell.Management

# Melihat execution policy.
Get-ExecutionPolicy -List

# Melihat lokasi profile PowerShell.
$PROFILE
```

### 5.3 PowerShell Remoting

PowerShell Remoting memakai WinRM.

Remoting memungkinkan admin menjalankan command di komputer lain tanpa RDP. Ini lebih efisien untuk administrasi banyak host karena output tetap object PowerShell. Pada domain, authentication biasanya lebih mulus dengan Kerberos. Pada workgroup/non-domain, perlu konfigurasi tambahan seperti TrustedHosts atau HTTPS listener.

Konsep:

| Item | Arti |
|---|---|
| WinRM | Windows Remote Management |
| PSSession | session remote persistent |
| Invoke-Command | menjalankan command remote |
| CredSSP/Kerberos/NTLM | opsi authentication tergantung environment |
| TrustedHosts | daftar host trusted untuk skenario non-domain tertentu |

Alur remoting:

```text
Client PowerShell
-> WinRM client
-> network TCP 5985/5986
-> WinRM service remote
-> authentication
-> session dibuat
-> command dieksekusi remote
-> object hasil dikirim balik
```

Failure mode:

| Gejala | Kemungkinan |
|---|---|
| `WinRM cannot complete operation` | listener/firewall/network |
| Access denied | credential/group/policy/UAC remote |
| Kerberos gagal | DNS/SPN/time/domain issue |
| Double-hop problem | credential tidak bisa dipakai ke server ketiga |

Command remoting:

```powershell
# Mengaktifkan PowerShell Remoting.
Enable-PSRemoting -Force

# Menguji WinRM lokal.
Test-WSMan localhost

# Menjalankan command di komputer remote.
Invoke-Command -ComputerName SERVER01 -ScriptBlock { hostname }

# Membuat session remote.
New-PSSession -ComputerName SERVER01

# Masuk ke session interaktif remote.
Enter-PSSession -ComputerName SERVER01
```

### 5.4 Scripting Basics

Script PowerShell sebaiknya jelas, bisa diuji, dan tidak diam-diam mengubah sistem tanpa validasi.

Script admin harus dianggap sebagai perubahan sistem. Script yang bagus punya input jelas, output jelas, error handling, logging, dan sebisa mungkin mendukung dry-run lewat `-WhatIf`. Untuk command yang berdampak besar, hindari hardcode dan tampilkan target yang akan diubah.

Prinsip:

| Prinsip | Arti |
|---|---|
| Idempotent | dijalankan berkali-kali hasilnya tetap aman |
| Explicit target | target host/file/user jelas |
| Validate input | input dicek sebelum dipakai |
| Error handling | error tidak diam-diam hilang |
| Logging | tindakan penting tercatat |
| Rollback | ada cara kembali |

Variable:

```powershell
# Menyimpan nama service ke variable.
$serviceName = "Spooler"
```

If:

```powershell
# Restart service hanya jika statusnya running.
if ((Get-Service -Name Spooler).Status -eq "Running") {
    # Restart service Spooler.
    Restart-Service -Name Spooler
}
```

Function:

```powershell
# Membuat function sederhana untuk membaca status service.
function Get-ServiceState {
    # Menerima nama service sebagai parameter.
    param(
        [string]$Name
    )

    # Mengambil status service dan menampilkan field penting.
    Get-Service -Name $Name | Select-Object Name, Status, StartType
}
```

Error handling:

```powershell
# Menangkap error command dengan try/catch.
try {
    # Mencari service dan berhenti jika terjadi error.
    Get-Service -Name "ServiceTidakAda" -ErrorAction Stop
} catch {
    # Menampilkan pesan error yang lebih jelas.
    Write-Error "Service tidak ditemukan: $($_.Exception.Message)"
}
```

---

## 6.0 Troubleshooting and Operations

### 6.1 Event Logs and Evidence

Saat troubleshooting Windows, kumpulkan bukti sebelum mengubah sistem.

Troubleshooting yang baik dimulai dari evidence, bukan tebakan. Windows punya banyak tempat untuk melihat bukti: Event Viewer, service status, process list, performance counter, update history, firewall log, Defender log, dan dump file. Kalau langsung mengubah setting tanpa mencatat kondisi awal, kamu bisa kehilangan jejak root cause.

Pertanyaan awal:

| Pertanyaan | Tujuan |
|---|---|
| Siapa yang terdampak | satu user, semua user, satu host, banyak host |
| Kapan mulai terjadi | cari hubungan dengan update/change |
| Apa error persisnya | bedakan access denied, timeout, crash, not found |
| Apa yang berubah | update, driver, policy, aplikasi, password |
| Apakah bisa direproduce | menentukan intermittent atau konsisten |
| Apa bukti paling dekat | event ID, log aplikasi, dump, counter |

Evidence checklist:

| Evidence | Command/Tool |
|---|---|
| OS version/build | `Get-ComputerInfo` |
| Recent system events | `Get-WinEvent -LogName System` |
| Failed logon | Security event `4625` |
| Service install | System event `7045` |
| Process/resource | Task Manager, `Get-Process` |
| Network path | `Test-NetConnection`, `tracert` |
| Updates | `Get-HotFix` |

Evidence hierarchy:

| Bukti | Kekuatan |
|---|---|
| Event log dengan timestamp | kuat |
| Command output saat masalah terjadi | kuat |
| Screenshot error | sedang |
| User report tanpa detail | awal, perlu verifikasi |
| Dugaan tanpa bukti | lemah |

Timeline:

```text
09:00 update driver terpasang
09:10 user mulai melihat Wi-Fi disconnect
09:12 System log mencatat adapter reset
09:20 rollback driver dilakukan
09:25 koneksi stabil
```

Command evidence:

```powershell
# Melihat ringkasan OS.
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber

# Melihat event critical/error System terbaru.
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2} -MaxEvents 20

# Melihat update terpasang.
Get-HotFix
```

### 6.2 Performance and Resource Troubleshooting

Resource utama:

Performance issue harus dipisah menjadi CPU, memory, disk, network, GPU, atau aplikasi. Jangan langsung menyimpulkan "Windows lambat". Lambat saat boot, lambat membuka aplikasi, lambat login, lambat network share, dan lambat browser bisa punya penyebab yang berbeda.

| Resource | Gejala Jika Bermasalah |
|---|---|
| CPU | aplikasi lambat, fan tinggi, queue |
| Memory | paging tinggi, app hang |
| Disk | latency tinggi, boot/app lambat |
| Network | transfer lambat, timeout |
| GPU | UI/render issue |

Cara membaca resource:

| Resource | Counter/Gejala | Interpretasi |
|---|---|---|
| CPU | `% Processor Time` tinggi | CPU sibuk |
| CPU | queue panjang | thread menunggu CPU |
| Memory | available rendah | memory pressure |
| Memory | paging tinggi | RAM kurang atau leak |
| Disk | avg sec/transfer tinggi | storage latency |
| Disk | queue tinggi | disk bottleneck |
| Network | retransmission/timeout | network/path/firewall |

Contoh diagnosis:

```text
User bilang "laptop lambat".

Jika CPU tinggi: cari process pemakai CPU.
Jika memory penuh: cari working set/commit besar.
Jika disk latency tinggi: cek update, antivirus scan, disk health.
Jika hanya app tertentu: cek app log/profile/network dependency.
```

Command performance:

```powershell
# Melihat process dengan CPU tertinggi.
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, Id, CPU

# Melihat process dengan memory tertinggi.
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10 Name, Id, WorkingSet

# Melihat counter CPU.
Get-Counter '\Processor(_Total)\% Processor Time'

# Melihat counter memory available.
Get-Counter '\Memory\Available MBytes'

# Melihat counter disk latency.
Get-Counter '\PhysicalDisk(_Total)\Avg. Disk sec/Transfer'
```

### 6.3 Boot, Recovery, dan Repair

Recovery tool:

Boot dan repair Windows harus dilakukan bertahap. SFC memperbaiki protected system files, sedangkan DISM memperbaiki component store yang dipakai SFC sebagai sumber. Jika component store rusak, SFC bisa gagal atau tidak menyelesaikan masalah. `chkdsk` fokus pada filesystem, bukan komponen Windows.

| Tool | Fungsi |
|---|---|
| Safe Mode | boot minimal untuk troubleshooting |
| WinRE | recovery environment |
| System Restore | rollback restore point |
| Startup Repair | repair boot tertentu |
| SFC | repair system files |
| DISM | repair Windows image/component store |

Kapan pakai apa:

| Gejala | Tool Awal |
|---|---|
| File sistem corrupt | SFC |
| SFC gagal repair | DISM lalu SFC ulang |
| Update gagal berulang | DISM, CBS log, Windows Update log |
| Filesystem error | chkdsk |
| Boot gagal | WinRE, Startup Repair, BCD |
| Driver baru membuat boot loop | Safe Mode, rollback driver |

Urutan repair umum:

```text
1. Backup data penting jika memungkinkan.
2. Catat error/event.
3. DISM health check.
4. DISM restore health jika perlu.
5. SFC scan.
6. Reboot.
7. Validasi event dan gejala.
```

Repair command:

```powershell
# Memeriksa dan memperbaiki system file.
sfc /scannow

# Memeriksa component store.
DISM /Online /Cleanup-Image /CheckHealth

# Scan component store lebih dalam.
DISM /Online /Cleanup-Image /ScanHealth

# Repair component store.
DISM /Online /Cleanup-Image /RestoreHealth

# Memeriksa disk filesystem.
chkdsk C: /scan
```

### 6.4 Network Troubleshooting

Network troubleshooting Windows harus menggabungkan IP, route, DNS, firewall, dan aplikasi.

Masalah network di Windows sering terlihat seperti satu gejala sederhana, misalnya "tidak bisa akses server". Padahal jalurnya berlapis: adapter, IP, route, DNS, firewall lokal, firewall network, port service, authentication, dan permission aplikasi. Karena itu troubleshooting harus memotong layer satu per satu.

Flow:

1. Cek link/interface.
2. Cek IP, mask, gateway, DNS.
3. Cek route ke destination.
4. Cek DNS resolution.
5. Cek TCP port.
6. Cek firewall/proxy/app.

Mapping gejala:

| Gejala | Kemungkinan |
|---|---|
| Tidak ada IP | DHCP/adapter/VLAN/Wi-Fi |
| IP `169.254.x.x` | DHCP gagal |
| Ping gateway gagal | local link/subnet/gateway |
| Ping IP bisa, nama gagal | DNS |
| DNS bisa, port timeout | firewall/service/routing |
| Port open, login gagal | credential/permission/auth |
| Hanya SMB gagal | TCP 445/firewall/share/NTFS |

Test berurutan:

```text
Get-NetAdapter
-> ipconfig /all
-> Get-NetRoute
-> Resolve-DnsName
-> Test-NetConnection -Port
-> Event Viewer / firewall / app log
```

Command network:

```powershell
# Melihat adapter.
Get-NetAdapter

# Melihat IP address.
Get-NetIPAddress

# Melihat route ke default gateway.
Get-NetRoute -DestinationPrefix 0.0.0.0/0

# Query DNS.
Resolve-DnsName example.com

# Test TCP port.
Test-NetConnection example.com -Port 443

# Trace route.
tracert example.com

# Melihat koneksi TCP aktif.
Get-NetTCPConnection
```

---

## 7.0 Senior Deep Dive

### 7.1 Windows Internals Overview

Windows internals membantu troubleshooting yang tidak cukup dilihat dari GUI.

Internals bukan berarti harus menjadi kernel developer. Untuk administrator, internals membantu membaca kenapa masalah terjadi: process berjalan sebagai siapa, service punya token apa, handle mana yang mengunci file, driver apa yang berjalan di kernel, dan event apa yang dibuat oleh komponen sistem.

Mental model:

```text
User menjalankan aplikasi
-> process dibuat
-> thread dijadwalkan CPU
-> process membawa token security
-> process membuka handle ke file/registry/socket
-> I/O masuk ke kernel/driver
-> event/counter/log bisa tercipta
```

Komponen:

| Komponen | Fungsi |
|---|---|
| Process | container eksekusi program |
| Thread | unit scheduling |
| Handle | referensi ke object kernel |
| Token | security context |
| Object Manager | mengelola object kernel |
| I/O Manager | request I/O |
| Memory Manager | virtual memory |
| Configuration Manager | registry |
| Service Control Manager | service lifecycle |

Useful concept:

| Concept | Arti |
|---|---|
| Session 0 isolation | service tidak berbagi desktop user |
| Integrity level | low/medium/high/system |
| UAC split token | admin punya filtered token sampai elevated |
| WOW64 | layer aplikasi 32-bit pada OS 64-bit |
| ETW | event tracing for Windows |

Kenapa berguna:

| Kasus | Internals yang Membantu |
|---|---|
| File tidak bisa dihapus | handle terbuka oleh process |
| Access denied padahal admin | token/UAC/integrity level |
| BSOD setelah update | driver kernel |
| Service jalan tapi app gagal | service account, dependency, port |
| Malware persistence | autoruns location, scheduled task, service |

### 7.2 Services, Sessions, Handles, dan Tokens

Service berjalan dalam context tertentu.

Windows membedakan session user interaktif dan session service. Sejak Session 0 isolation, service tidak berjalan di desktop user seperti aplikasi biasa. Ini mengurangi risiko security, tetapi juga membuat service tidak boleh bergantung pada interaksi GUI user.

Handle adalah referensi process ke object seperti file, registry key, event, mutex, process, thread, atau socket. Jika file tidak bisa dihapus karena "in use", biasanya ada process yang masih memegang handle ke file tersebut.

Token menentukan hak process. Dua process milik user yang sama bisa punya kemampuan berbeda jika satu elevated dan satu tidak.

Service account:

| Account | Arti |
|---|---|
| LocalSystem | privilege sangat tinggi lokal |
| LocalService | privilege rendah lokal |
| NetworkService | privilege rendah dengan network identity computer |
| Custom service account | account khusus service |
| gMSA | managed service account domain, dibahas di AD |

Service account trade-off:

| Account | Kelebihan | Risiko |
|---|---|---|
| LocalSystem | sangat kuat, mudah akses lokal | blast radius besar jika service compromise |
| LocalService | privilege rendah | mungkin kurang hak untuk resource tertentu |
| NetworkService | bisa akses network sebagai computer account | perlu paham identity network |
| Custom account | permission bisa spesifik | password/rotation harus dikelola |
| gMSA | password dikelola domain | butuh AD, dibahas nanti |

Token analysis:

```powershell
# Melihat token group dan privilege user saat ini.
whoami /all

# Melihat process dan owner via CIM.
Get-CimInstance Win32_Process | Select-Object -First 10 ProcessId, Name
```

### 7.3 ETW, Sysmon, dan Sysinternals

ETW adalah mekanisme event tracing low-level Windows. Sysinternals adalah toolkit troubleshooting Microsoft yang sangat penting untuk admin senior.

ETW dipakai oleh banyak komponen Windows untuk mencatat event berperforma tinggi. Banyak log modern di Event Viewer sebenarnya bersumber dari provider ETW. Sysmon memanfaatkan mekanisme logging untuk memberi event security yang lebih berguna, misalnya process create, network connection, driver loaded, dan file creation.

Sysinternals membantu saat log bawaan tidak cukup:

- Process Explorer untuk melihat process tree, DLL, handle, token.
- Process Monitor untuk melihat file/registry/process/network event real-time.
- Autoruns untuk melihat lokasi auto-start/persistence.
- TCPView untuk melihat koneksi network aktif.

Sysinternals tool:

| Tool | Fungsi |
|---|---|
| Process Explorer | process, handle, DLL, token |
| Process Monitor | file/registry/process/network event |
| Autoruns | startup/persistence location |
| TCPView | TCP/UDP connection GUI |
| PsExec | remote process execution |
| Handle | mencari process yang memegang file/handle |
| Sigcheck | cek signature/hash |

Sysmon:

| Event | Arti |
|---|---|
| Process Create | process baru |
| Network Connection | koneksi network |
| File Create Time Changed | timestomping clue |
| Driver Loaded | driver load |
| Image Loaded | DLL load |

Catatan:

- Sysmon perlu config yang baik agar signal tidak terlalu berisik.
- Process Monitor sangat kuat, tetapi capture bisa besar; filter dulu sebelum investigasi panjang.
- Sysinternals sebaiknya diunduh dari Microsoft Sysinternals resmi.

Contoh penggunaan:

| Masalah | Tool |
|---|---|
| File terkunci | Process Explorer / Handle |
| App gagal karena registry missing | Process Monitor |
| Startup lambat | Autoruns |
| Koneksi mencurigakan | TCPView |
| Binary tidak signed | Sigcheck |

### 7.4 Operational Runbook

Template incident Windows:

Runbook adalah panduan langkah kerja saat masalah terjadi. Runbook yang baik tidak hanya berisi "restart service", tetapi juga evidence yang perlu dikumpulkan, kondisi rollback, cara validasi, dan kapan eskalasi. Tujuannya supaya incident bisa ditangani konsisten meskipun orang yang menangani berbeda.

| Field | Isi |
|---|---|
| Scope | host/user/service yang terdampak |
| Symptom | error, event ID, timeout, crash |
| Start time | kapan mulai |
| Last change | update, policy, driver, software |
| Evidence | event log, command output, screenshot, dump |
| Theory | dugaan penyebab |
| Action | perubahan yang dilakukan |
| Validation | bukti pulih |
| Rollback | cara kembali |

Runbook yang baik:

| Bagian | Pertanyaan |
|---|---|
| Scope | siapa/apa yang terdampak |
| Preconditions | akses/tool apa yang dibutuhkan |
| Evidence | log/counter/output apa yang harus disimpan |
| Action | langkah yang dilakukan |
| Decision point | kapan lanjut, rollback, atau eskalasi |
| Validation | bukti service pulih |
| Documentation | apa yang dicatat setelah selesai |

Contoh runbook service down:

```text
1. Cek status service.
2. Cek dependency service.
3. Cek System/Application log.
4. Cek account service dan permission.
5. Cek port/listening socket jika service network.
6. Start service.
7. Validasi aplikasi.
8. Dokumentasikan event ID dan tindakan.
```

### 7.5 Security Descriptor, ACL, ACE, dan Integrity Level

Windows security object tidak hanya "permission read/write". Hampir semua securable object seperti file, registry key, service, process, thread, share, printer, dan scheduled task punya security descriptor.

Security descriptor adalah metadata keamanan yang menempel pada object. Saat process ingin membuka object, Windows mengecek token process terhadap security descriptor object tersebut. Ini inti dari banyak kasus "access denied".

Alur access check:

```text
Process punya access token
-> process meminta akses ke object
-> Windows membaca security descriptor object
-> deny ACE dicek
-> allow ACE dicek
-> integrity level dicek
-> privilege khusus bisa memengaruhi hasil
-> access granted atau denied
```

Security descriptor:

| Komponen | Fungsi |
|---|---|
| Owner | pemilik object |
| Primary group | legacy/POSIX-like compatibility, jarang disentuh langsung |
| DACL | discretionary access control list, menentukan allow/deny |
| SACL | system access control list, menentukan audit |
| Control flags | inheritance/protection dan metadata security descriptor |

Contoh kasus:

```text
User Bob masuk group Users.
Folder C:\Data memberi Users read-only.
Bob mencoba menulis file baru.
Hasil: access denied, karena token Bob hanya match read permission.
```

Kalau Bob adalah local administrator tetapi terminalnya tidak elevated, ia tetap bisa gagal mengubah area sistem karena token yang dipakai adalah filtered token.

ACE:

| Field | Arti |
|---|---|
| Type | allow, deny, audit |
| Principal/SID | user/group/security principal |
| Access mask | hak seperti read, write, execute, delete |
| Inheritance flag | apakah ACE diwariskan ke child object |
| Object flag | scope ACE pada object tertentu |

DACL vs SACL:

| ACL | Fungsi |
|---|---|
| DACL | menentukan siapa boleh/ditolak melakukan operasi |
| SACL | menentukan operasi mana yang dicatat ke audit log |

Mandatory Integrity Control:

| Integrity Level | Contoh |
|---|---|
| Low | browser sandbox, protected mode |
| Medium | aplikasi user normal |
| High | elevated administrator |
| System | service/system process |

Catatan penting:

- DACL menjawab "boleh atau tidak".
- Integrity level menjawab "apakah level caller cukup tinggi".
- Process low integrity tidak bisa menulis ke object medium integrity walau DACL terlihat mengizinkan.
- SACL butuh privilege khusus untuk dibaca/diubah.

SDDL ringkas:

```text
O:BAG:SYD:(A;;FA;;;BA)(A;;RX;;;BU)
```

Field SDDL:

| Bagian | Arti |
|---|---|
| `O:` | owner |
| `G:` | primary group |
| `D:` | DACL |
| `S:` | SACL |
| `A` | allow ACE |
| `FA` | full access |
| `RX` | read and execute |
| `BA` | Built-in Administrators |
| `BU` | Built-in Users |

Privilege yang sering penting:

| Privilege | Arti | Risiko |
|---|---|---|
| SeDebugPrivilege | debug/open process lain | bisa membaca process sensitif |
| SeBackupPrivilege | bypass read ACL untuk backup | bisa membaca data luas |
| SeRestorePrivilege | restore/write dengan bypass tertentu | bisa mengganti file sensitif |
| SeImpersonatePrivilege | impersonate client token | sering relevan dalam privilege escalation |
| SeSecurityPrivilege | manage audit/security log tertentu | akses SACL/audit |
| SeTakeOwnershipPrivilege | mengambil ownership | bisa mengubah akses setelah ownership |

Command security descriptor:

```powershell
# Melihat ACL folder dengan PowerShell.
Get-Acl C:\Data | Format-List

# Melihat ACL folder dengan icacls.
icacls C:\Data

# Melihat group, SID, privilege, dan integrity level user saat ini.
whoami /all

# Melihat audit policy lokal.
auditpol /get /category:*

# Melihat security descriptor service dengan sc.exe.
sc.exe sdshow Spooler
```

### 7.6 LSASS, Authentication Flow, dan Credential Protection

LSASS adalah process inti security Windows. Ia menangani local security policy, logon session, token creation, authentication package, dan banyak operasi credential.

Saat user login, Windows perlu membuktikan identitas user dan membuat token. LSASS berada di pusat proses ini. Karena LSASS berkaitan dengan credential dan token, ia menjadi komponen yang sangat sensitif. Banyak kontrol keamanan modern berusaha melindungi credential agar tidak mudah dicuri dari memory atau disk.

Logon flow lokal sederhana:

```text
User memasukkan credential
-> Winlogon/LogonUI
-> LSASS
-> authentication package
-> SAM/local policy validation
-> access token dibuat
-> user session dimulai
```

Perbedaan local dan domain:

| Login | Validasi |
|---|---|
| Local account | SAM lokal |
| Microsoft account | cloud/Microsoft service |
| Domain account online | domain controller, biasanya Kerberos |
| Domain account offline | cached credential jika tersedia |

Kenapa time penting:

- Kerberos sangat sensitif terhadap selisih waktu.
- Token dan ticket punya masa berlaku.
- Event timeline akan kacau jika jam host salah.

Komponen authentication:

| Komponen | Fungsi |
|---|---|
| LSASS | Local Security Authority Subsystem Service |
| SAM | local account database |
| LSA secrets | secret lokal tertentu |
| DPAPI | proteksi secret berbasis user/machine |
| Credential Manager | penyimpanan credential user |
| Kerberos | authentication domain, detail di AD |
| NTLM | challenge-response legacy, masih muncul pada banyak skenario |

Logon type penting pada event `4624`:

| Type | Arti |
|---:|---|
| 2 | interactive logon |
| 3 | network logon |
| 4 | batch logon |
| 5 | service logon |
| 7 | unlock |
| 10 | remote interactive/RDP |
| 11 | cached interactive |

Credential protection:

| Fitur | Fungsi |
|---|---|
| Credential Guard | isolasi secret tertentu dengan virtualization-based security |
| LSA protection | menjalankan LSASS sebagai protected process light jika didukung/dikonfigurasi |
| Windows Hello | auth modern berbasis key/biometric/PIN |
| BitLocker | melindungi disk offline |
| Defender ASR | mengurangi teknik pencurian credential tertentu |

Command authentication:

```powershell
# Melihat identity dan privilege user saat ini.
whoami /all

# Melihat credential yang tersimpan user.
cmdkey /list

# Melihat Kerberos ticket jika host domain-joined.
klist

# Melihat failed logon event.
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 20

# Melihat successful logon event.
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 20
```

Catatan:

- Jangan dump LSASS pada production kecuali ada proses incident response yang sah dan disetujui.
- Credential Guard/LSA protection bisa memengaruhi compatibility tool lama.
- Investigasi credential theft harus menjaga chain of custody dan bukti.

### 7.7 Windows Servicing, Component Store, dan Image Health

Windows servicing bukan sekadar "install update". Di belakangnya ada component store, CBS, DISM, Windows Update agent, driver store, dan pending operations.

Component store (`WinSxS`) menyimpan komponen Windows yang dipakai untuk update, repair, dan enable/disable feature. DISM memperbaiki image/component store. SFC memakai sumber yang sehat untuk memperbaiki protected system files. Karena itu urutan DISM lalu SFC sering dipakai ketika sistem dicurigai corrupt.

Komponen servicing:

| Komponen | Fungsi |
|---|---|
| WinSxS | component store Windows |
| CBS | Component-Based Servicing |
| DISM | service/repair image Windows |
| SFC | validasi dan repair protected system files |
| Windows Update | download/install update |
| Servicing Stack | komponen yang menginstal update |
| Driver Store | repository driver |

DISM vs SFC:

| Tool | Fokus |
|---|---|
| DISM | kesehatan image/component store |
| SFC | protected system files |
| Windows Update | source update/repair jika online |
| CBS log | detail operasi servicing |

Log penting:

| Log/File | Isi |
|---|---|
| `C:\Windows\Logs\CBS\CBS.log` | detail component servicing |
| `C:\Windows\Logs\DISM\dism.log` | aktivitas DISM |
| WindowsUpdate event log | update scan/download/install |
| Setup log | upgrade/feature update |

Repair order umum:

```text
1. Cek event/update history.
2. Jalankan DISM health check.
3. Repair component store jika perlu.
4. Jalankan SFC.
5. Reboot jika ada pending operation.
6. Validasi update/app/service.
```

Command servicing:

```powershell
# Cek apakah component store terdeteksi bermasalah.
DISM /Online /Cleanup-Image /CheckHealth

# Scan component store lebih dalam.
DISM /Online /Cleanup-Image /ScanHealth

# Repair component store.
DISM /Online /Cleanup-Image /RestoreHealth

# Validasi dan repair protected system files.
sfc /scannow

# Melihat hotfix terpasang.
Get-HotFix

# Membuat Windows Update log dari ETL.
Get-WindowsUpdateLog
```

Pending reboot clue:

| Clue | Arti |
|---|---|
| update meminta reboot | file/registry operation pending |
| CBS pending | servicing belum selesai |
| driver install pending | driver aktif setelah reboot |
| rename/delete pending | file terkunci saat runtime |

### 7.8 WMI, CIM, WinRM, dan Remote Operations

WMI/CIM adalah lapisan management Windows yang sangat luas. Banyak inventory, monitoring, EDR, backup agent, dan admin tooling bergantung pada ini.

WMI/CIM menyediakan cara standar untuk membaca dan mengelola informasi Windows: OS, process, service, disk, network, event, BIOS, dan banyak lagi. Banyak tool enterprise tidak membaca semuanya sendiri; mereka query WMI/CIM provider. Jika WMI bermasalah, monitoring dan inventory bisa ikut rusak.

WMI vs CIM:

| Item | WMI | CIM |
|---|---|---|
| Era | legacy Windows management | standard modern |
| PowerShell lama | `Get-WmiObject` | `Get-CimInstance` |
| Transport | DCOM | WSMan/WinRM |
| Rekomendasi modern | legacy/support | gunakan CIM jika memungkinkan |

Cara membayangkan CIM:

```text
Namespace seperti folder
Class seperti tipe object
Instance seperti object nyata
Property seperti field
Method seperti aksi
```

Contoh:

```text
Namespace: root\cimv2
Class:     Win32_OperatingSystem
Instance:  OS Windows yang sedang berjalan
Property:  LastBootUpTime
```

Namespace dan class:

| Item | Contoh | Arti |
|---|---|---|
| Namespace | `root\cimv2` | container class |
| Class | `Win32_OperatingSystem` | schema object |
| Instance | satu OS aktif | data nyata |
| Property | `LastBootUpTime` | field data |
| Method | reboot, start, stop | aksi |

Command CIM:

```powershell
# Mencari class CIM terkait operating system.
Get-CimClass -ClassName *OperatingSystem*

# Melihat informasi OS via CIM.
Get-CimInstance Win32_OperatingSystem

# Melihat process via CIM.
Get-CimInstance Win32_Process

# Membuat CIM session remote.
New-CimSession -ComputerName SERVER01

# Query OS lewat CIM session.
Get-CimInstance Win32_OperatingSystem -CimSession SERVER01
```

Failure mode WMI/CIM:

| Gejala | Dugaan |
|---|---|
| access denied | privilege, UAC remote restriction, firewall, namespace ACL |
| RPC unavailable | DCOM/firewall/service/network |
| WinRM timeout | listener/firewall/auth/trusted hosts |
| query lambat | provider bermasalah atau repository issue |
| inventory kosong | namespace/class tidak ada atau agent rusak |

### 7.9 Crash Dump, Reliability, dan Driver Failure

BSOD dan crash berat biasanya butuh dump, event, driver timeline, dan update history. Jangan hanya membaca stop code dari layar.

Blue screen terjadi ketika Windows menemukan kondisi fatal di kernel mode dan memutuskan berhenti untuk mencegah kerusakan lebih lanjut. Stop code berguna sebagai petunjuk awal, tetapi root cause sering perlu dump, driver list, update timeline, dan event log.

Reliability Monitor membantu melihat pola: kapan crash mulai terjadi, aplikasi apa yang gagal, update apa yang terpasang, dan apakah masalah muncul setelah perubahan tertentu.

Dump type:

| Dump | Isi | Catatan |
|---|---|---|
| Small memory dump | ringkasan crash | `C:\Windows\Minidump` |
| Kernel memory dump | kernel memory | cukup untuk banyak driver crash |
| Complete memory dump | seluruh memory | besar, perlu pagefile cukup |
| Active memory dump | memory aktif | lebih efisien pada server besar |

Cara membaca evidence:

| Evidence | Ditanyakan |
|---|---|
| Stop code | jenis crash umum |
| Faulting module | driver/module yang dicurigai |
| Timestamp | kapan mulai |
| Recent update | apakah driver/update baru masuk |
| Hardware event | disk/memory/NIC error |
| Reproducible | bisa dipicu ulang atau random |

Evidence BSOD:

| Evidence | Lokasi/Command |
|---|---|
| Minidump | `C:\Windows\Minidump` |
| Full dump | `C:\Windows\MEMORY.DMP` |
| BugCheck event | System event `1001` |
| Unexpected shutdown | System event `6008` |
| Reliability Monitor | `perfmon /rel` |
| Driver list | `driverquery` atau `Get-WindowsDriver` |

Command crash/reliability:

```powershell
# Membuka Reliability Monitor.
perfmon /rel

# Melihat dump kecil.
Get-ChildItem C:\Windows\Minidump -ErrorAction SilentlyContinue

# Melihat MEMORY.DMP jika ada.
Get-Item C:\Windows\MEMORY.DMP -ErrorAction SilentlyContinue

# Melihat BugCheck event.
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1001} -MaxEvents 10

# Melihat unexpected shutdown event.
Get-WinEvent -FilterHashtable @{LogName='System'; Id=6008} -MaxEvents 10

# Melihat driver dengan driverquery.
driverquery /v

# Melihat Driver Verifier setting.
verifier /querysettings
```

Driver Verifier:

- Berguna untuk menangkap driver bug.
- Bisa membuat sistem crash lebih cepat agar penyebab terlihat.
- Jangan aktifkan sembarangan pada production tanpa recovery plan.
- Siapkan Safe Mode/WinRE untuk disable jika boot loop.

### 7.10 Expert Troubleshooting Playbooks

Expert troubleshooting selalu dimulai dari scope dan evidence. Tujuannya bukan mencoba semua command, tetapi mempersempit failure domain.

Access denied playbook:

```text
1. Identifikasi object: file, registry, service, share, process.
2. Cek user SID, group, privilege, integrity level.
3. Cek DACL, deny ACE, inheritance, owner.
4. Cek apakah proses elevated atau medium integrity.
5. Cek SACL/audit event jika aktif.
6. Validasi dengan akun/group yang tepat.
```

Service tidak bisa start:

```text
1. Cek status service dan dependency.
2. Cek service account.
3. Cek permission file, registry, dan log path.
4. Cek port conflict jika service network.
5. Cek System/Application event.
6. Cek update/driver/change terakhir.
7. Jalankan start manual dan capture error.
```

ProcMon workflow:

```text
1. Set filter Process Name atau Path.
2. Reproduce issue secepat mungkin.
3. Stop capture.
4. Cari Result seperti ACCESS DENIED, NAME NOT FOUND, PATH NOT FOUND.
5. Baca stack jika perlu symbol.
6. Export PML/CSV hanya bagian relevan.
```

Autoruns workflow:

```text
1. Jalankan sebagai administrator.
2. Hide Microsoft entries jika mencari persistence pihak ketiga.
3. Cek Logon, Services, Drivers, Scheduled Tasks, Winsock Providers.
4. Verifikasi signature dan path.
5. Disable dulu sebelum delete jika belum yakin.
```

Expert command set:

```powershell
# Melihat startup folder user.
Get-ChildItem "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup" -Force

# Melihat Run key HKCU.
Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Run

# Melihat Run key HKLM.
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Run

# Melihat service yang automatic.
Get-Service | Where-Object StartType -eq Automatic

# Melihat scheduled task yang bukan disabled.
Get-ScheduledTask | Where-Object State -ne Disabled

# Melihat process beserta path jika tersedia.
Get-Process | Select-Object Name, Id, Path

# Melihat TCP listener.
Get-NetTCPConnection -State Listen
```

---

## Lab Checklist

- Cek apakah host workgroup atau domain joined.
- Buat local user dan local group.
- Beri NTFS permission ke folder lab.
- Buat SMB share dan uji akses.
- Cek Event Viewer untuk logon gagal.
- Restart service dan lihat event terkait.
- Buat scheduled task sederhana.
- Tambahkan IP static sementara dan rollback.
- Flush DNS cache dan test `Resolve-DnsName`.
- Enable PowerShell Remoting di VM lab.
- Jalankan `sfc` dan `DISM` check.
- Cek Defender status dan update signature.
- Export local security policy.
- Ambil baseline process, service, firewall rule, dan update.
- Baca security descriptor folder dengan `Get-Acl` dan `icacls`.
- Bandingkan token user biasa dan elevated dengan `whoami /all`.
- Cari event logon type `2`, `3`, dan `10` di Security log.
- Generate Windows Update log dengan `Get-WindowsUpdateLog`.
- Query OS/process lewat CIM.
- Buka Reliability Monitor dan cek crash/app failure.
- Simulasikan service gagal start di VM lab dan buat runbook.
- Gunakan ProcMon filter untuk mencari `ACCESS DENIED`.
- Gunakan Autoruns untuk review startup location.

---

## Command Index Cepat

```powershell
# Melihat informasi komputer.
Get-ComputerInfo

# Melihat user lokal.
Get-LocalUser

# Melihat group lokal.
Get-LocalGroup

# Melihat process.
Get-Process

# Melihat service.
Get-Service

# Melihat scheduled task.
Get-ScheduledTask

# Melihat disk.
Get-Disk

# Melihat volume.
Get-Volume

# Melihat IP address.
Get-NetIPAddress

# Melihat route.
Get-NetRoute

# Resolve DNS.
Resolve-DnsName example.com

# Test TCP port.
Test-NetConnection example.com -Port 443

# Melihat firewall profile.
Get-NetFirewallProfile

# Melihat event System terbaru.
Get-WinEvent -LogName System -MaxEvents 20

# Melihat Defender status.
Get-MpComputerStatus

# Melihat BitLocker status.
Get-BitLockerVolume

# Melihat PowerShell help.
Get-Help Get-Service -Examples

# Melihat ACL folder.
Get-Acl C:\Data

# Melihat ACL dengan icacls.
icacls C:\Data

# Melihat token dan privilege user.
whoami /all

# Melihat audit policy.
auditpol /get /category:*

# Melihat credential yang tersimpan.
cmdkey /list

# Melihat event failed logon.
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 10

# Query OS lewat CIM.
Get-CimInstance Win32_OperatingSystem

# Cek image health.
DISM /Online /Cleanup-Image /CheckHealth

# Repair system files.
sfc /scannow

# Membuka Reliability Monitor.
perfmon /rel

# Melihat minidump jika ada.
Get-ChildItem C:\Windows\Minidump -ErrorAction SilentlyContinue
```

---

## Checklist Kesiapan Praktik

Catatan ini sudah berguna kalau kamu bisa:

- membedakan Windows client, Windows Server, workgroup, domain, dan local account
- menjelaskan boot flow Windows modern
- membaca path penting seperti `System32`, `SysWOW64`, `ProgramData`, dan user profile
- membaca dan backup registry key
- mengelola process, service, dan scheduled task
- membuat local user/group dan membaca group membership
- menjelaskan NTFS permission, share permission, inheritance, deny, dan UAC
- membaca disk, partition, volume, drive letter, dan BitLocker status
- membaca IP, route, DNS cache, neighbor table, dan firewall profile
- menguji port dengan `Test-NetConnection`
- membaca event log penting seperti `4624`, `4625`, `7045`, `6008`
- memakai PowerShell pipeline berbasis object
- menjalankan basic repair dengan `sfc`, `DISM`, dan `chkdsk`
- membuat incident note dengan scope, evidence, theory, action, validation, dan rollback
- menjelaskan security descriptor, DACL, SACL, ACE, SID, privilege, dan integrity level
- membedakan token user biasa, elevated admin, dan service context
- membaca logon type Windows dan event authentication penting
- memahami LSASS, SAM, DPAPI, Credential Manager, dan credential protection secara konseptual
- memahami component store, CBS, DISM, SFC, dan Windows Update log
- memakai WMI/CIM untuk inventory dan troubleshooting remote
- mengumpulkan evidence BSOD dari minidump, event `1001`, event `6008`, dan Reliability Monitor
- memakai Sysinternals secara metodis: Process Explorer, ProcMon, Autoruns, TCPView
