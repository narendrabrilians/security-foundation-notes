# Windows Server

Sumber resmi utama:

- Windows Server documentation: https://learn.microsoft.com/en-us/windows-server/
- Windows Server management: https://learn.microsoft.com/en-us/windows-server/administration/manage-windows-server
- Server Manager: https://learn.microsoft.com/en-us/windows-server/administration/server-manager/server-manager
- Add or remove roles and features: https://learn.microsoft.com/en-us/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features
- Windows Server security: https://learn.microsoft.com/en-us/windows-server/security/security-and-assurance
- DNS Server: https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-overview
- DHCP Server: https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top
- SMB features: https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-feature-descriptions
- Detect, enable, and disable SMBv1, SMBv2, and SMBv3: https://learn.microsoft.com/en-us/windows-server/storage/file-server/troubleshoot/detect-enable-and-disable-smbv1-v2-v3
- Hyper-V: https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/hyper-v-on-windows-server
- Failover Clustering: https://learn.microsoft.com/en-us/windows-server/failover-clustering/clustering-requirements
- Storage Spaces Direct: https://learn.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview

Area utama:

| Area | Cakupan |
|---|---|
| Fundamentals | edition, Server Core, roles, features, identity, time, baseline build |
| Deployment | install, initial configuration, RSAT, Server Manager, Windows Admin Center, patching |
| Network Services | static IP, DNS Server, DHCP Server, NTP, remote access, NPS overview |
| Storage and File | NTFS, ReFS, SMB, shares, DFS, FSRM, iSCSI, Storage Spaces |
| Workloads | IIS, TLS binding, RDS overview, print server, app compatibility |
| Virtualization | Hyper-V, virtual switch, VM storage, checkpoints, containers |
| High Availability | failover cluster, quorum, witness, CAU, Storage Spaces Direct |
| Security | baseline, firewall, Defender, service accounts, credential protection, audit |
| Troubleshooting | event logs, performance counters, network, storage, update, recovery |
| AD Bridge | member server, domain join, DNS dependency, GPO impact |

## Daftar Isi

- [1.0 Windows Server Fundamentals](#10-windows-server-fundamentals)
  - [1.1 Posisi Windows Server di Infrastruktur](#11-posisi-windows-server-di-infrastruktur)
  - [1.2 Editions, Installation Options, dan Licensing Concepts](#12-editions-installation-options-dan-licensing-concepts)
  - [1.3 Server Core, Desktop Experience, dan Management Tools](#13-server-core-desktop-experience-dan-management-tools)
  - [1.4 Roles, Role Services, Features, dan Payload](#14-roles-role-services-features-dan-payload)
  - [1.5 Naming, Time, Network Identity, dan Baseline Build](#15-naming-time-network-identity-dan-baseline-build)
- [2.0 Deployment and Lifecycle](#20-deployment-and-lifecycle)
  - [2.1 Installation, Initial Configuration, dan Golden Image](#21-installation-initial-configuration-dan-golden-image)
  - [2.2 Server Manager, Windows Admin Center, dan RSAT](#22-server-manager-windows-admin-center-dan-rsat)
  - [2.3 PowerShell Administration and Remote Management](#23-powershell-administration-and-remote-management)
  - [2.4 Patch Management and Maintenance Windows](#24-patch-management-and-maintenance-windows)
  - [2.5 Backup, Restore, VSS, dan Disaster Recovery](#25-backup-restore-vss-dan-disaster-recovery)
- [3.0 Network Services](#30-network-services)
  - [3.1 Static IP, NIC, Route, dan DNS Client](#31-static-ip-nic-route-dan-dns-client)
  - [3.2 DNS Server](#32-dns-server)
  - [3.3 DHCP Server](#33-dhcp-server)
  - [3.4 Time Service and NTP](#34-time-service-and-ntp)
  - [3.5 Remote Access, VPN, dan NPS Overview](#35-remote-access-vpn-dan-nps-overview)
- [4.0 Storage and File Services](#40-storage-and-file-services)
  - [4.1 Disk, Volume, NTFS, ReFS, dan Mount Point](#41-disk-volume-ntfs-refs-dan-mount-point)
  - [4.2 SMB File Server](#42-smb-file-server)
  - [4.3 Share Permission, NTFS Permission, dan Access Based Enumeration](#43-share-permission-ntfs-permission-dan-access-based-enumeration)
  - [4.4 DFS Namespace and DFS Replication](#44-dfs-namespace-and-dfs-replication)
  - [4.5 FSRM Quota, File Screening, dan Classification](#45-fsrm-quota-file-screening-dan-classification)
  - [4.6 iSCSI, MPIO, Storage Spaces, dan Data Deduplication](#46-iscsi-mpio-storage-spaces-dan-data-deduplication)
- [5.0 Web, App, and Remote Desktop Services](#50-web-app-and-remote-desktop-services)
  - [5.1 IIS Architecture and Hosting](#51-iis-architecture-and-hosting)
  - [5.2 TLS Certificate, Binding, dan App Pool Identity](#52-tls-certificate-binding-dan-app-pool-identity)
  - [5.3 Remote Desktop Services Overview](#53-remote-desktop-services-overview)
  - [5.4 Print Server and Application Compatibility](#54-print-server-and-application-compatibility)
- [6.0 Virtualization and Containers](#60-virtualization-and-containers)
  - [6.1 Hyper-V Architecture](#61-hyper-v-architecture)
  - [6.2 Virtual Switch, VLAN, dan VM Network](#62-virtual-switch-vlan-dan-vm-network)
  - [6.3 VM Storage, Checkpoints, dan Integration Services](#63-vm-storage-checkpoints-dan-integration-services)
  - [6.4 Windows Containers Overview](#64-windows-containers-overview)
- [7.0 High Availability and Scale](#70-high-availability-and-scale)
  - [7.1 Failover Clustering Concepts](#71-failover-clustering-concepts)
  - [7.2 Quorum, Witness, Cluster Network, dan CSV](#72-quorum-witness-cluster-network-dan-csv)
  - [7.3 Clustered Workloads and Cluster Aware Updating](#73-clustered-workloads-and-cluster-aware-updating)
  - [7.4 Storage Spaces Direct](#74-storage-spaces-direct)
  - [7.5 Load Balancing and Resilience Patterns](#75-load-balancing-and-resilience-patterns)
- [8.0 Security and Hardening](#80-security-and-hardening)
  - [8.1 Security Baseline and Local Policy](#81-security-baseline-and-local-policy)
  - [8.2 Defender, Firewall, and Attack Surface Reduction](#82-defender-firewall-and-attack-surface-reduction)
  - [8.3 Service Accounts, Privilege, and Secrets](#83-service-accounts-privilege-and-secrets)
  - [8.4 Credential Protection and Remote Admin Security](#84-credential-protection-and-remote-admin-security)
  - [8.5 Logging, Auditing, and Detection](#85-logging-auditing-and-detection)
- [9.0 Monitoring and Troubleshooting](#90-monitoring-and-troubleshooting)
  - [9.1 Event Logs, Reliability, and Data Teknis](#91-event-logs-reliability-and-data-teknis)
  - [9.2 Performance Monitor and Counters](#92-performance-monitor-and-counters)
  - [9.3 Network Troubleshooting](#93-network-troubleshooting)
  - [9.4 Storage and File Server Troubleshooting](#94-storage-and-file-server-troubleshooting)
  - [9.5 Boot, Update, and Component Store Repair](#95-boot-update-and-component-store-repair)
- [10.0 Active Directory Bridge](#100-active-directory-bridge)
  - [10.1 Member Server vs Domain Controller](#101-member-server-vs-domain-controller)
  - [10.2 Domain Join and DNS Dependency](#102-domain-join-and-dns-dependency)
  - [10.3 Group Policy Impact on Servers](#103-group-policy-impact-on-servers)
  - [10.4 What Moves to Active-Directory.md](#104-what-moves-to-active-directorymd)

---

## 1.0 Windows Server Fundamentals

**Fokus teknis:** identifikasi role server, management plane, dan service dependency.

```powershell
# Tampilkan informasi OS server.
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber

# Tampilkan role dan feature yang terpasang.
Get-WindowsFeature | Where-Object Installed

# Tampilkan service running.
Get-Service | Where-Object Status -eq Running
```

Aspek teknis: role utama, feature pendukung, service dependency, port yang mungkin terbuka, dan cara admin mengelola server.

### 1.1 Posisi Windows Server di Infrastruktur

Windows Server adalah OS server untuk menjalankan workload yang dipakai banyak client. Workload itu bisa berupa identity, file sharing, DNS, DHCP, web server, virtualization, remote desktop, certificate service, update service, atau aplikasi bisnis.

Perbedaan penting antara Windows client dan Windows Server bukan hanya tampilan. Windows Server punya model role dan feature, service background yang lebih banyak, konfigurasi remote management, opsi Server Core, dukungan workload enterprise, dan ekspektasi availability yang lebih tinggi.

| Item | Fungsi |
|---|---|
| Role | fungsi utama server, misalnya DNS Server, DHCP Server, Hyper-V, File Server |
| Role service | bagian spesifik dari role, misalnya Web Server IIS punya subkomponen HTTP, logging, ASP.NET |
| Feature | kemampuan tambahan OS, misalnya Failover Clustering, BitLocker, .NET Framework |
| Workload | service/aplikasi yang benar-benar dipakai bisnis |
| Dependency | komponen yang dibutuhkan workload, misalnya DNS, storage, certificate, identity |
| Management plane | jalur administrasi seperti WinRM, RDP, WAC, MMC, PowerShell |
| Data plane | jalur traffic service seperti SMB, DNS, DHCP, HTTP, RDP, SQL |

Contoh role umum:

| Role | Port umum | Fungsi |
|---|---:|---|
| DNS Server | TCP/UDP 53 | name resolution |
| DHCP Server | UDP 67/68 | automatic IP assignment |
| File Server SMB | TCP 445 | file sharing |
| IIS | TCP 80/443 | web service |
| Hyper-V | bervariasi | virtualization host |
| RDS | TCP/UDP 3389 dan komponen tambahan | remote desktop/session hosting |
| AD DS | banyak port | identity domain, dibahas detail di AD |

Cara berpikir server:

| Pertanyaan | Kenapa penting |
|---|---|
| Service apa yang disediakan | menentukan role, port, dependency, dan monitoring |
| Siapa client-nya | menentukan firewall, subnet, permission, dan SLA |
| Data apa yang disimpan | menentukan backup, encryption, audit, dan recovery |
| Bagaimana admin masuk | menentukan RDP, WinRM, jump host, dan least privilege |
| Bagaimana patching dilakukan | menentukan maintenance window dan rollback |
| Bagaimana failure ditangani | menentukan HA, backup, restore, dan DR |

```powershell
# Tampilkan informasi OS, edition, versi, dan build server.
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber, OsArchitecture

# Tampilkan nama komputer dan domain/workgroup membership.
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain, Workgroup

# Tampilkan role dan feature yang sudah terinstall.
Get-WindowsFeature | Where-Object Installed
```

### 1.2 Editions, Installation Options, dan Licensing Concepts

Windows Server biasanya dipilih berdasarkan workload, hak virtualisasi, dan fitur enterprise yang diperlukan. Secara umum:

| Edition | Cocok untuk | Keterangan |
|---|---|---|
| Standard | workload fisik/VM jumlah terbatas | umum untuk server kecil dan menengah |
| Datacenter | virtualization density tinggi, S2D, software-defined datacenter | cocok untuk host Hyper-V besar dan cluster |
| Essentials | small business scenario tertentu | ketersediaan/fitur bergantung versi |
| Azure Edition | workload tertentu di Azure/Azure Stack HCI/Azure Local | bukan default on-prem biasa |

Konsep licensing yang perlu dipahami:

| Konsep | Arti |
|---|---|
| Core-based licensing | lisensi server dihitung berdasarkan core fisik |
| CAL | Client Access License untuk user/device yang mengakses service Windows Server |
| VM rights | hak menjalankan VM Windows Server di host tertentu |
| Activation | proses aktivasi OS, bisa retail, MAK, KMS, atau subscription |
| Evaluation | versi trial untuk lab, harus direncanakan sebelum produksi |

Installation option:

| Option | Ciri |
|---|---|
| Server Core | tanpa desktop shell penuh, footprint lebih kecil, attack surface lebih rendah |
| Desktop Experience | punya GUI penuh, lebih familiar, footprint lebih besar |
| Nano Server | container image scenario, bukan general purpose server login |

Untuk produksi, Server Core sering lebih baik untuk role yang bisa dikelola remote, misalnya Hyper-V, DNS, DHCP, file server, dan domain controller. Desktop Experience masuk akal untuk admin yang masih membutuhkan GUI lokal atau aplikasi yang memang butuh shell/GUI.

```powershell
# Cek edition dan channel instalasi Windows Server.
DISM /Online /Get-CurrentEdition

# Cek edition target yang mungkin tersedia untuk upgrade edition.
DISM /Online /Get-TargetEditions

# Cek status aktivasi Windows.
slmgr /dli
```

### 1.3 Server Core, Desktop Experience, dan Management Tools

Server Core tidak berarti server tidak bisa dikelola. Server Core justru mendorong pola administrasi yang lebih sehat: remote management, PowerShell, RSAT, Server Manager, Windows Admin Center, dan automation.

Perbedaan operasional:

| Area | Server Core | Desktop Experience |
|---|---|---|
| GUI lokal | minimal | penuh |
| Attack surface | lebih kecil | lebih besar |
| Patch footprint | cenderung lebih kecil | lebih besar |
| Admin habit | remote dan scripted | GUI dan lokal lebih mudah |
| Cocok untuk | server role stabil | lab, legacy app, admin GUI-heavy |

Management tools:

| Tool | Fungsi |
|---|---|
| SConfig | konfigurasi dasar Server Core |
| PowerShell | konfigurasi dan automation |
| Server Manager | manage role dan server pool dari GUI remote |
| RSAT | MMC/tools dari Windows client |
| Windows Admin Center | web-based server management |
| MMC snap-ins | DNS Manager, DHCP Manager, Event Viewer, Computer Management |

Hal penting: jangan menjadikan RDP sebagai satu-satunya cara manage server. RDP berguna, tetapi WinRM/PowerShell lebih scalable, mudah diaudit, dan cocok untuk automation.

```powershell
# Buka konfigurasi dasar Server Core.
sconfig

# Cek apakah WinRM aktif untuk remote management.
Get-Service WinRM

# Aktifkan PowerShell remoting pada server.
Enable-PSRemoting -Force

# Cek firewall rule bawaan untuk Windows Remote Management.
Get-NetFirewallRule -DisplayGroup "Windows Remote Management" | Select-Object DisplayName, Enabled, Profile
```

### 1.4 Roles, Role Services, Features, dan Payload

Windows Server memakai konsep role dan feature supaya satu OS bisa menjadi banyak jenis server. Role adalah fungsi server utama. Feature adalah kapabilitas OS tambahan yang mendukung role atau kebutuhan administrasi.

Contoh:

| Kebutuhan | Role atau Feature |
|---|---|
| File sharing | File and Storage Services |
| DNS internal | DNS Server |
| IP leasing | DHCP Server |
| Web hosting | Web Server IIS |
| Virtualization | Hyper-V |
| HA cluster | Failover Clustering feature |
| Remote management tools | RSAT feature |

Payload berarti file instalasi komponen yang diperlukan untuk menambahkan role/feature. Pada beberapa environment, source payload bisa berasal dari image, Windows Update, atau source path internal.

Operasional penting:

- install hanya role yang dibutuhkan
- dokumentasikan role owner dan purpose
- hindari mencampur workload kritis yang tidak berkaitan
- pisahkan role berisiko tinggi
- review service dan firewall setelah role diinstall
- buat baseline sebelum dan sesudah role ditambah

```powershell
# Lihat semua role dan feature beserta statusnya.
Get-WindowsFeature

# Cari role atau feature yang terkait DNS.
Get-WindowsFeature *DNS*

# Install role DNS Server beserta management tools.
Install-WindowsFeature DNS -IncludeManagementTools

# Uninstall feature yang tidak lagi dipakai.
Uninstall-WindowsFeature Telnet-Client
```

### 1.5 Naming, Time, Network Identity, dan Baseline Build

Server harus punya identitas yang jelas. Nama server bukan kosmetik; nama dipakai di DNS, monitoring, certificate, inventory, backup, dan incident response.

Contoh pola nama:

| Pola | Contoh | Arti |
|---|---|---|
| lokasi-role-nomor | `JKT-FS-01` | Jakarta file server 01 |
| environment-role-nomor | `PRD-DNS-01` | production DNS 01 |
| app-tier-nomor | `ERP-WEB-01` | ERP web tier 01 |

Time juga kritis. Authentication, certificate validation, log correlation, scheduled tasks, Kerberos, dan distributed systems sangat bergantung pada waktu yang konsisten. Di domain, domain member biasanya sync waktu dari domain hierarchy. Di workgroup/lab, tentukan NTP source yang jelas.

Baseline build adalah kondisi minimum server sebelum workload dipasang:

| Area | Baseline |
|---|---|
| Identity | hostname benar, domain/workgroup benar |
| Network | static IP untuk server infra, DNS benar |
| Time | timezone dan NTP benar |
| Patch | update security terpasang |
| Security | firewall aktif, admin terbatas |
| Logging | audit dasar aktif, log size cukup |
| Backup | backup policy jelas |
| Monitoring | agent/counter/event subscription siap |

```powershell
# Rename server sebelum role produksi dipasang.
Rename-Computer -NewName "LAB-FS-01" -Restart

# Tampilkan timezone saat ini.
Get-TimeZone

# Set timezone server.
Set-TimeZone -Id "SE Asia Standard Time"

# Cek sumber sinkronisasi waktu.
w32tm /query /source

# Cek status detail Windows Time.
w32tm /query /status
```

---

## 2.0 Deployment and Lifecycle

**Fokus teknis:** perlakukan build server sebagai state yang bisa diverifikasi.

```powershell
# Melihat hotfix/patch yang terpasang.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 10

# Melihat konfigurasi waktu.
w32tm /query /status

# Melihat firewall profile.
Get-NetFirewallProfile
```

Aspek teknis: patch level, waktu/NTP source, baseline firewall, perubahan terakhir, dan rollback plan sebelum perubahan besar.

### 2.1 Installation, Initial Configuration, dan Golden Image

Deployment server bukan hanya "install OS". Deployment yang baik menghasilkan server yang konsisten, bisa diaudit, bisa dipulihkan, dan mudah dikelola.

Tahap umum:

1. Tentukan role dan owner.
2. Pilih edition dan installation option.
3. Set hostname, IP, DNS, timezone.
4. Patch OS.
5. Aktifkan remote management.
6. Terapkan baseline security.
7. Install role.
8. Konfigurasi monitoring dan backup.
9. Dokumentasikan port, dependency, dan rollback.

Golden image adalah image dasar yang sudah berisi baseline umum, tetapi belum berisi secret, identity unik, atau konfigurasi workload spesifik. Hindari cloning server tanpa sysprep karena SID, hostname, certificate, agent ID, dan identitas lain bisa bentrok.

Checklist initial configuration:

| Item | Verifikasi |
|---|---|
| Hostname | sesuai naming standard |
| IP address | static atau DHCP reservation sesuai desain |
| DNS client | mengarah ke resolver internal yang benar |
| Time | timezone dan NTP benar |
| Remote admin | WinRM/RDP sesuai policy |
| Local admin | password unik, LAPS jika domain |
| Firewall | aktif, rule minimum |
| Updates | security patch terpasang |
| Monitoring | server muncul di monitoring |

```powershell
# Tampilkan konfigurasi IP lengkap.
Get-NetIPConfiguration

# Tampilkan adapter jaringan fisik dan virtual.
Get-NetAdapter | Sort-Object Name

# Cek update hotfix yang terinstall.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20

# Cek apakah server menunggu restart setelah perubahan sistem.
Test-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending"
```

### 2.2 Server Manager, Windows Admin Center, dan RSAT

Server Manager adalah console bawaan untuk mengelola server lokal dan remote. Windows Admin Center adalah console web modern untuk manage server, cluster, Hyper-V, storage, event, services, registry, firewall, dan extension lain. RSAT menyediakan tool admin seperti DNS Manager, DHCP Manager, Group Policy Management, dan AD tools di Windows client.

Pola kerja yang baik:

| Skenario | Tool |
|---|---|
| Install role di beberapa server | Server Manager atau PowerShell |
| Manage server core dari browser | Windows Admin Center |
| Manage DNS/DHCP dengan GUI | RSAT MMC |
| Automation massal | PowerShell remoting |
| Emergency console | RDP atau console hypervisor |

Jangan install semua GUI tools di semua server. Lebih baik admin workstation punya RSAT, sedangkan server hanya menjalankan role yang diperlukan.

```powershell
# Cek module ServerManager.
Get-Command -Module ServerManager

# Tampilkan daftar server yang bisa dikelola dari Server Manager local profile.
Get-ChildItem "$env:APPDATA\Microsoft\Windows\ServerManager" -ErrorAction SilentlyContinue

# Install RSAT DNS tool pada Windows client yang mendukung capability.
Add-WindowsCapability -Online -Name Rsat.Dns.Tools~~~~0.0.1.0

# Install RSAT DHCP tool pada Windows client yang mendukung capability.
Add-WindowsCapability -Online -Name Rsat.DHCP.Tools~~~~0.0.1.0
```

### 2.3 PowerShell Administration and Remote Management

PowerShell adalah alat utama untuk Windows Server modern. Yang penting bukan hanya tahu command, tetapi paham object pipeline, remoting, idempotency, dan error handling.

Remote management utama:

| Teknologi | Port | Fungsi |
|---|---:|---|
| WinRM HTTP | TCP 5985 | PowerShell remoting default internal |
| WinRM HTTPS | TCP 5986 | remoting terenkripsi TLS |
| RDP | TCP/UDP 3389 | interactive desktop |
| SMB admin shares | TCP 445 | file/admin operations |
| WMI/DCOM legacy | TCP 135 dan dynamic RPC | management lama |

PowerShell Remoting memakai WinRM. Di domain, authentication biasanya lebih mulus karena Kerberos. Di workgroup, perlu TrustedHosts atau HTTPS listener supaya tidak bergantung pada trust domain.

Konsep penting:

| Konsep | Arti |
|---|---|
| one-to-one | masuk ke session remote interaktif |
| one-to-many | menjalankan command ke banyak server |
| implicit remoting | import command dari session remote |
| JEA | Just Enough Administration untuk membatasi capability admin |
| constrained endpoint | endpoint remoting dengan role capability terbatas |

```powershell
# Aktifkan PowerShell remoting pada server.
Enable-PSRemoting -Force

# Test koneksi WinRM ke server remote.
Test-WSMan -ComputerName LAB-FS-01

# Jalankan command sederhana di server remote.
Invoke-Command -ComputerName LAB-FS-01 -ScriptBlock { hostname }

# Masuk ke PowerShell session interaktif server remote.
Enter-PSSession -ComputerName LAB-FS-01

# Buat persistent session untuk menjalankan beberapa command.
$s = New-PSSession -ComputerName LAB-FS-01

# Hapus persistent session setelah selesai.
Remove-PSSession $s
```

### 2.4 Patch Management and Maintenance Windows

Patch management di server harus direncanakan karena reboot server bisa memutus layanan. Bedakan antara patch security, cumulative update, driver/firmware update, aplikasi, dan update role tertentu.

Komponen patching:

| Komponen | Fungsi |
|---|---|
| Windows Update | mekanisme update bawaan |
| WSUS | update approval dan distribution on-prem |
| Configuration Manager | endpoint/server management enterprise |
| Azure Update Manager | patch orchestration hybrid/cloud |
| Cluster Aware Updating | patch node cluster bergiliran |
| Maintenance window | waktu perubahan yang disetujui |

Prinsip patching server:

- test di lab atau staging sebelum produksi
- pastikan backup dan rollback tersedia
- cek dependency workload
- patch node HA secara bergiliran
- monitor event log setelah reboot
- catat KB number dan waktu instalasi

```powershell
# Tampilkan update terakhir berdasarkan hotfix.
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 15

# Cek service Windows Update.
Get-Service wuauserv

# Cek reboot pending dari Component Based Servicing.
Test-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending"

# Cek reboot pending dari Windows Update Auto Update.
Test-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired"
```

### 2.5 Backup, Restore, VSS, dan Disaster Recovery

Backup bukan hanya punya file cadangan. Backup harus bisa direstore. Disaster recovery bukan hanya restore file; DR mencakup urutan pemulihan service, dependency, identity, DNS, certificate, database, storage, dan network.

Konsep backup:

| Konsep | Arti |
|---|---|
| RPO | berapa banyak data boleh hilang |
| RTO | berapa lama service boleh down |
| full backup | backup lengkap |
| incremental | backup perubahan sejak backup terakhir |
| bare metal recovery | restore seluruh server ke hardware/VM |
| system state | komponen OS penting, registry, boot files, dan role tertentu |
| VSS | snapshot consistency mechanism di Windows |

VSS punya writer yang bertanggung jawab membuat aplikasi dalam kondisi konsisten saat snapshot. Jika VSS writer rusak, backup bisa selesai tetapi tidak application-consistent.

```powershell
# Cek status VSS writers.
vssadmin list writers

# Cek daftar shadow copies.
vssadmin list shadows

# Cek fitur Windows Server Backup.
Get-WindowsFeature Windows-Server-Backup

# Install Windows Server Backup.
Install-WindowsFeature Windows-Server-Backup

# Tampilkan command wbadmin yang tersedia.
wbadmin /?
```

Runbook restore minimum:

| Tahap | Pertanyaan |
|---|---|
| Identifikasi | data/service apa yang hilang |
| Scope | satu file, satu volume, satu server, atau site |
| Backup source | backup mana yang valid dan paling dekat RPO |
| Dependency | butuh DNS, AD, certificate, storage, network |
| Restore | restore ke lokasi asli atau alternate |
| Validation | client bisa akses, log bersih, data konsisten |
| Postmortem | penyebab dan pencegahan |

---

## 3.0 Network Services

**Fokus teknis:** uji DNS, DHCP, dan konektivitas service dari server side.

```powershell
# Query DNS A record.
Resolve-DnsName fs01.lab.local

# Query SRV record domain controller.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Test koneksi SMB ke file server.
Test-NetConnection fs01.lab.local -Port 445
```

Aspek teknis: DNS server yang menjawab, record yang ditemukan, port service, firewall path, dan dependency domain.

### 3.1 Static IP, NIC, Route, dan DNS Client

Server infrastruktur biasanya memakai static IP atau DHCP reservation. DHCP biasa untuk server produksi berisiko jika lease berubah, DNS client salah, atau server tidak mendapat IP saat boot.

Komponen network server:

| Komponen | Fungsi |
|---|---|
| IP address | identitas layer 3 |
| subnet mask/prefix | menentukan network lokal |
| default gateway | route keluar subnet |
| DNS client server | resolver yang dipakai server |
| route table | keputusan forwarding traffic keluar |
| interface metric | prioritas interface |
| firewall profile | Domain, Private, Public |

DNS client di server sangat penting. Untuk domain member, DNS biasanya harus mengarah ke DNS internal yang tahu zone AD, bukan DNS publik langsung. DNS publik bisa dipakai sebagai forwarder di DNS server, bukan sebagai DNS client utama domain member.

```powershell
# Tampilkan konfigurasi IP semua interface.
Get-NetIPConfiguration

# Set IP static pada interface bernama Ethernet.
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 10.10.10.20 -PrefixLength 24 -DefaultGateway 10.10.10.1

# Set DNS client server address.
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.10.10.10,10.10.10.11

# Tampilkan route table IPv4.
Get-NetRoute -AddressFamily IPv4 | Sort-Object RouteMetric

# Test koneksi TCP ke port SMB server lain.
Test-NetConnection 10.10.10.30 -Port 445
```

### 3.2 DNS Server

DNS Server menerjemahkan nama menjadi IP dan menyimpan zone DNS. Di lingkungan Windows, DNS juga menjadi dependency utama Active Directory karena client memakai DNS untuk menemukan domain controller melalui record SRV.

Konsep DNS Server:

| Konsep | Arti |
|---|---|
| zone | database untuk domain DNS tertentu |
| forward lookup zone | nama ke IP |
| reverse lookup zone | IP ke nama |
| A record | nama ke IPv4 |
| AAAA record | nama ke IPv6 |
| CNAME | alias ke nama lain |
| MX | mail exchanger |
| SRV | service location, penting untuk AD |
| NS | authoritative name server |
| SOA | metadata authority zone |
| forwarder | DNS upstream untuk query eksternal |
| conditional forwarder | forward query domain tertentu ke DNS tertentu |
| recursion | DNS server membantu client mencari jawaban non-authoritative |
| scavenging | pembersihan record dinamis lama |

Jenis zone:

| Jenis | Fungsi |
|---|---|
| Primary | writable copy utama |
| Secondary | read-only copy dari primary via zone transfer |
| Stub | berisi record minimal untuk menemukan authoritative server |
| AD-integrated | zone disimpan dan direplikasi lewat AD |

Security DNS:

- batasi zone transfer hanya ke server yang perlu
- jangan membuka recursive resolver internal ke internet
- gunakan conditional forwarder untuk domain partner/internal
- aktifkan logging secukupnya saat troubleshooting
- pahami dynamic update, terutama jika AD-integrated
- dokumentasikan forwarder dan root hints behavior

```powershell
# Install role DNS Server.
Install-WindowsFeature DNS -IncludeManagementTools

# Tampilkan zone DNS di server lokal.
Get-DnsServerZone

# Buat forward lookup zone primary.
Add-DnsServerPrimaryZone -Name "lab.local" -ZoneFile "lab.local.dns"

# Tambahkan A record untuk file server.
Add-DnsServerResourceRecordA -ZoneName "lab.local" -Name "fs01" -IPv4Address 10.10.10.20

# Query record dari DNS server tertentu.
Resolve-DnsName fs01.lab.local -Server 10.10.10.10

# Tampilkan forwarder DNS.
Get-DnsServerForwarder

# Set forwarder DNS ke resolver upstream.
Set-DnsServerForwarder -IPAddress 1.1.1.1,8.8.8.8
```

Troubleshooting DNS:

| Gejala | Cek |
|---|---|
| client tidak resolve nama internal | DNS client server, zone, record, firewall TCP/UDP 53 |
| FQDN bisa tapi short name gagal | DNS suffix search list |
| record lama masih muncul | TTL, cache client, cache server, scavenging |
| external lookup lambat | forwarder/root hints unreachable |
| AD login lambat | SRV record dan DNS DC locator |

```powershell
# Flush DNS client cache pada server/client.
Clear-DnsClientCache

# Tampilkan DNS client cache.
Get-DnsClientCache | Select-Object -First 20

# Query SOA record untuk zone.
Resolve-DnsName lab.local -Type SOA -Server 10.10.10.10

# Query SRV record domain controller untuk domain lab.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV -Server 10.10.10.10

# Cek event log DNS Server terbaru.
Get-WinEvent -LogName "DNS Server" -MaxEvents 20
```

### 3.3 DHCP Server

DHCP Server memberikan IP address dan konfigurasi network ke client secara otomatis. DHCP bukan hanya "bagi IP"; DHCP juga memberi default gateway, DNS server, DNS suffix, lease duration, PXE options, dan opsi lain.

Alur DHCP IPv4:

| Tahap | Paket | Arti |
|---|---|---|
| 1 | Discover | client mencari DHCP server |
| 2 | Offer | server menawarkan IP |
| 3 | Request | client meminta IP yang ditawarkan |
| 4 | Acknowledge | server menyetujui lease |

Konsep penting:

| Konsep | Arti |
|---|---|
| scope | range IP yang disewakan |
| exclusion | IP dalam scope yang tidak boleh diberikan |
| reservation | IP tetap berdasarkan MAC/client ID |
| lease duration | durasi IP dipinjam |
| option 003 | default gateway |
| option 006 | DNS servers |
| option 015 | DNS domain name |
| DHCP relay | meneruskan broadcast DHCP antar subnet |
| split scope | pembagian scope untuk redundansi sederhana |
| DHCP failover | high availability DHCP antar dua server |

DHCP server di domain AD biasanya harus authorized agar tidak ada rogue DHCP. Rogue DHCP bisa menyebabkan client mendapat gateway/DNS salah dan memutus jaringan.

```powershell
# Install DHCP Server role.
Install-WindowsFeature DHCP -IncludeManagementTools

# Tampilkan scope DHCP.
Get-DhcpServerv4Scope

# Buat DHCP scope IPv4.
Add-DhcpServerv4Scope -Name "LAB-CLIENTS" -StartRange 10.10.10.100 -EndRange 10.10.10.200 -SubnetMask 255.255.255.0

# Set default gateway untuk scope.
Set-DhcpServerv4OptionValue -ScopeId 10.10.10.0 -Router 10.10.10.1

# Set DNS server dan DNS suffix untuk scope.
Set-DhcpServerv4OptionValue -ScopeId 10.10.10.0 -DnsServer 10.10.10.10 -DnsDomain "lab.local"

# Tampilkan lease aktif pada scope.
Get-DhcpServerv4Lease -ScopeId 10.10.10.0
```

Troubleshooting DHCP:

| Gejala | Kemungkinan |
|---|---|
| client dapat APIPA 169.254.x.x | DHCP tidak terjangkau atau scope habis |
| client dapat IP tapi DNS salah | option 006 salah |
| client beda subnet tidak dapat IP | DHCP relay/IP helper tidak ada |
| beberapa client dapat gateway aneh | rogue DHCP |
| scope penuh | lease duration terlalu panjang atau range kurang |

```powershell
# Cek statistik DHCP server.
Get-DhcpServerv4Statistics

# Cek scope yang hampir penuh.
Get-DhcpServerv4ScopeStatistics

# Tampilkan audit log DHCP path default.
Get-ChildItem "C:\Windows\System32\dhcp" -Filter "DhcpSrvLog-*.log"

# Restart service DHCP Server setelah perubahan terencana.
Restart-Service DHCPServer
```

### 3.4 Time Service and NTP

Waktu yang benar adalah dependency untuk authentication, certificate, log correlation, replication, scheduled task, backup, dan troubleshooting. Dalam domain, domain member mengikuti hierarchy waktu domain. PDC Emulator di forest root biasanya menjadi sumber waktu utama domain dan harus sinkron ke NTP eksternal yang terpercaya.

Konsep:

| Komponen | Fungsi |
|---|---|
| W32Time | service Windows Time |
| NTP | Network Time Protocol |
| time source | sumber waktu server |
| skew | selisih waktu |
| PDC Emulator | sumber waktu penting di domain |

Best practice:

- domain member sync ke domain hierarchy
- jangan set semua server domain langsung ke internet NTP
- monitor time skew
- korelasikan timezone dan UTC saat membaca log
- pastikan hypervisor time sync tidak konflik dengan domain time untuk VM tertentu

```powershell
# Cek sumber waktu saat ini.
w32tm /query /source

# Cek status waktu detail.
w32tm /query /status

# Tampilkan peer NTP.
w32tm /query /peers

# Resync waktu manual.
w32tm /resync

# Cek konfigurasi service Windows Time.
w32tm /query /configuration
```

### 3.5 Remote Access, VPN, dan NPS Overview

Remote Access di Windows Server bisa mencakup VPN, routing, DirectAccess legacy, dan Web Application Proxy. NPS atau Network Policy Server sering dipakai sebagai RADIUS server untuk Wi-Fi enterprise, VPN authentication, dan network access policy.

Konsep NPS:

| Komponen | Fungsi |
|---|---|
| RADIUS client | device yang meminta auth, misalnya VPN gateway atau wireless controller |
| RADIUS server | NPS yang memproses request |
| network policy | aturan siapa boleh akses |
| connection request policy | aturan routing request |
| shared secret | secret antara RADIUS client dan server |
| accounting | log koneksi |

Untuk security engineer, NPS penting karena menjadi titik kontrol akses jaringan. Salah policy bisa membuat user tidak bisa VPN atau justru memberi akses terlalu luas.

```powershell
# Install Network Policy and Access Services role.
Install-WindowsFeature NPAS -IncludeManagementTools

# Cek service Network Policy Server.
Get-Service IAS

# Tampilkan port UDP yang umum dipakai RADIUS.
Get-NetUDPEndpoint | Where-Object LocalPort -in 1812,1813,1645,1646

# Cek event log NPS terbaru.
Get-WinEvent -LogName "System" -MaxEvents 50 | Where-Object ProviderName -like "*NPS*"
```

---

## 4.0 Storage and File Services

**Fokus teknis:** verifikasi storage, share, NTFS ACL, dan akses client.

```powershell
# Tampilkan volume dan filesystem.
Get-Volume

# Tampilkan SMB share.
Get-SmbShare

# Tampilkan permission SMB share.
Get-SmbShareAccess

# Tampilkan ACL folder.
Get-Acl D:\Shares | Format-List
```

Aspek teknis: volume health, share path, share permission, NTFS permission, inheritance, dan perbedaan share ACL vs NTFS ACL.

### 4.1 Disk, Volume, NTFS, ReFS, dan Mount Point

Storage server harus dipahami dari layer bawah ke atas:

1. physical disk atau virtual disk
2. partition
3. volume
4. filesystem
5. folder/share
6. permission
7. application data

Konsep disk:

| Konsep | Arti |
|---|---|
| Basic disk | model disk umum |
| Dynamic disk | teknologi lama untuk volume dinamis |
| GPT | partition table modern |
| MBR | partition table lama dengan limit |
| NTFS | filesystem umum Windows |
| ReFS | filesystem modern untuk workload tertentu |
| allocation unit | ukuran cluster filesystem |
| mount point | volume dipasang sebagai folder |

NTFS cocok untuk general purpose, ACL detail, compression, EFS, dan banyak workload. ReFS cocok untuk beberapa scenario modern seperti Storage Spaces Direct dan workload virtualisasi tertentu, dengan fitur integrity dan resilience. Jangan memilih ReFS hanya karena lebih baru; cek dukungan aplikasi, backup, dedup, dan fitur yang dibutuhkan.

```powershell
# Tampilkan disk fisik/virtual yang terlihat OS.
Get-Disk

# Tampilkan volume dan filesystem.
Get-Volume

# Tampilkan partition.
Get-Partition

# Inisialisasi disk baru dengan GPT.
Initialize-Disk -Number 1 -PartitionStyle GPT

# Buat partition baru memakai seluruh disk dan assign drive letter.
New-Partition -DiskNumber 1 -UseMaximumSize -DriveLetter D

# Format volume sebagai NTFS.
Format-Volume -DriveLetter D -FileSystem NTFS -NewFileSystemLabel "Data"
```

### 4.2 SMB File Server

SMB adalah protocol file sharing utama Windows. File server bukan hanya folder yang dibagikan; ada kombinasi SMB dialect, share permission, NTFS ACL, signing, encryption, access-based enumeration, offline files, auditing, quota, backup, dan monitoring.

Konsep SMB:

| Konsep | Arti |
|---|---|
| SMB dialect | versi SMB yang dinegosiasikan client/server |
| share | namespace network seperti `\\server\share` |
| session | koneksi user/client ke server |
| open file | file yang sedang dibuka client |
| signing | integritas traffic SMB |
| encryption | enkripsi traffic SMB |
| multichannel | beberapa koneksi network untuk performa/resilience |
| SMB Direct | SMB memakai RDMA untuk latency rendah |
| CA share | continuously available share untuk cluster |

Security SMB:

- disable SMB1 kecuali ada alasan legacy sangat kuat
- require signing untuk environment yang membutuhkan proteksi relay/tampering
- gunakan SMB encryption untuk data sensitif atau network tidak terpercaya
- audit share sensitif
- hindari memberi `Everyone Full Control` tanpa desain NTFS ACL yang benar
- gunakan admin shares hanya untuk administrasi terbatas

```powershell
# Install File Server role.
Install-WindowsFeature FS-FileServer -IncludeManagementTools

# Tampilkan SMB shares.
Get-SmbShare

# Buat folder untuk share data.
New-Item -Path "D:\Shares\Finance" -ItemType Directory

# Buat SMB share baru.
New-SmbShare -Name "Finance" -Path "D:\Shares\Finance" -FullAccess "LAB\Domain Admins" -ChangeAccess "LAB\Finance"

# Tampilkan session SMB aktif.
Get-SmbSession

# Tampilkan file yang sedang dibuka lewat SMB.
Get-SmbOpenFile

# Cek konfigurasi SMB server.
Get-SmbServerConfiguration
```

### 4.3 Share Permission, NTFS Permission, dan Access Based Enumeration

Akses file server ditentukan oleh kombinasi share permission dan NTFS permission. Effective permission adalah hasil paling restriktif dari keduanya.

Layer permission:

| Layer | Fungsi |
|---|---|
| Share permission | membatasi akses saat masuk lewat network share |
| NTFS permission | membatasi akses di filesystem lokal dan network |
| Ownership | pemilik object yang bisa mengubah permission tertentu |
| Inheritance | ACL turun dari parent folder |
| Access Based Enumeration | user hanya melihat file/folder yang boleh diakses |

Model yang sering dipakai:

| Layer | Rekomendasi |
|---|---|
| Share | beri group luas `Change` atau `Full` sesuai standar |
| NTFS | kontrol detail memakai group role-based |
| User assignment | user masuk group, bukan ACL langsung |
| Admin | admin group terpisah dan diaudit |

Contoh struktur group:

| Group | Permission |
|---|---|
| `GG-Finance-Read` | Read |
| `GG-Finance-Modify` | Modify |
| `GG-Finance-Owner` | Full Control terbatas admin/data owner |

```powershell
# Lihat ACL folder.
Get-Acl "D:\Shares\Finance" | Format-List

# Disable inheritance dan copy permission yang ada.
icacls "D:\Shares\Finance" /inheritance:d

# Beri permission Modify ke group Finance.
icacls "D:\Shares\Finance" /grant "LAB\GG-Finance-Modify:(OI)(CI)M"

# Aktifkan access based enumeration pada share.
Set-SmbShare -Name "Finance" -FolderEnumerationMode AccessBased

# Tampilkan permission SMB share.
Get-SmbShareAccess -Name "Finance"
```

Simbol penting `icacls`:

| Simbol | Arti |
|---|---|
| `OI` | Object Inherit, turun ke file |
| `CI` | Container Inherit, turun ke folder |
| `F` | Full Control |
| `M` | Modify |
| `RX` | Read and Execute |
| `R` | Read |
| `W` | Write |

### 4.4 DFS Namespace and DFS Replication

DFS Namespace memberi path virtual seperti `\\domain.local\files\Finance` yang bisa menunjuk ke beberapa file server. DFS Replication mereplikasi data antar server. Keduanya berbeda walaupun sering dipakai bersama.

| Komponen | Fungsi |
|---|---|
| DFS Namespace | namespace/path logis |
| namespace server | server yang menyimpan namespace |
| folder target | target share nyata |
| DFS Replication | replikasi file antar folder |
| replication group | kumpulan server/folder yang direplikasi |
| staging folder | area sementara replikasi |
| backlog | antrian file yang belum direplikasi |

Hal yang harus hati-hati:

- DFSR bukan pengganti backup
- konflik file bisa terjadi jika banyak site menulis file yang sama
- staging size harus cukup
- replikasi awal data besar perlu direncanakan
- monitor backlog dan event log DFSR

```powershell
# Install DFS Namespace dan DFS Replication.
Install-WindowsFeature FS-DFS-Namespace, FS-DFS-Replication -IncludeManagementTools

# Tampilkan command DFSN yang tersedia.
Get-Command -Module DFSN

# Tampilkan command DFSR yang tersedia.
Get-Command -Module DFSR

# Cek event DFS Replication terbaru.
Get-WinEvent -LogName "DFS Replication" -MaxEvents 20
```

### 4.5 FSRM Quota, File Screening, dan Classification

File Server Resource Manager membantu mengontrol penggunaan storage. FSRM bisa membuat quota, file screening, laporan storage, dan classification rule.

Fitur FSRM:

| Fitur | Fungsi |
|---|---|
| quota | membatasi ukuran folder |
| soft quota | hanya alert, tidak memblokir |
| hard quota | memblokir saat limit |
| file screening | blokir tipe file tertentu |
| storage report | laporan file besar, duplikat, owner, quota |
| classification | memberi metadata berdasarkan rule |

Contoh penggunaan:

| Masalah | FSRM |
|---|---|
| user menyimpan ISO besar di share | file screening |
| folder departemen tumbuh tanpa kontrol | quota |
| cari file lama dan besar | storage report |
| data sensitif perlu label | classification |

```powershell
# Install FSRM.
Install-WindowsFeature FS-Resource-Manager -IncludeManagementTools

# Tampilkan command FSRM.
Get-Command -Module FileServerResourceManager

# Buat hard quota 100 GB pada folder share.
New-FsrmQuota -Path "D:\Shares\Finance" -Size 100GB

# Tampilkan quota yang ada.
Get-FsrmQuota

# Tampilkan file screen template.
Get-FsrmFileScreenTemplate
```

### 4.6 iSCSI, MPIO, Storage Spaces, dan Data Deduplication

Windows Server bisa menjadi iSCSI initiator dan juga iSCSI target. iSCSI membawa block storage lewat IP network. MPIO memberi multipath ke storage agar path failure tidak langsung memutus akses.

Konsep storage lanjutan:

| Konsep | Arti |
|---|---|
| iSCSI initiator | client yang mengakses LUN |
| iSCSI target | storage endpoint yang menyediakan LUN |
| LUN | logical unit block storage |
| MPIO | multipath I/O untuk redundancy/performance |
| Storage Spaces | pooling disk lokal |
| Storage Spaces Direct | pooling disk antar node cluster |
| Data Deduplication | menghemat space dengan menyimpan blok unik |

Hati-hati:

- jangan taruh iSCSI production di network yang sama tanpa QoS/desain
- MPIO harus dikonfigurasi sesuai storage vendor
- dedup tidak cocok untuk semua workload
- backup software harus mendukung layout storage
- ukur latency, IOPS, throughput, bukan hanya kapasitas

```powershell
# Cek iSCSI Initiator service.
Get-Service MSiSCSI

# Aktifkan iSCSI Initiator service.
Start-Service MSiSCSI

# Set iSCSI Initiator service start otomatis.
Set-Service MSiSCSI -StartupType Automatic

# Install Multipath I/O feature.
Install-WindowsFeature Multipath-IO

# Install Data Deduplication feature.
Install-WindowsFeature FS-Data-Deduplication

# Tampilkan physical disk untuk Storage Spaces.
Get-PhysicalDisk
```

---

## 5.0 Web, App, and Remote Desktop Services

**Fokus teknis:** validasi web/RDS dari service, binding, certificate, dan port.

```powershell
# Test akses HTTP lokal.
Invoke-WebRequest http://localhost -UseBasicParsing

# Test port HTTPS.
Test-NetConnection localhost -Port 443

# Tampilkan listener TCP.
Get-NetTCPConnection -State Listen
```

Aspek teknis: status code, binding, certificate, listening port, firewall rule, dan log service yang menunjukkan request diterima.

### 5.1 IIS Architecture and Hosting

IIS adalah web server Windows. Untuk admin dan security engineer, IIS harus dipahami dari sisi site, binding, application pool, worker process, module, logging, certificate, dan identity.

Komponen IIS:

| Komponen | Fungsi |
|---|---|
| site | unit website |
| binding | kombinasi IP, port, hostname, protocol |
| application pool | isolasi process aplikasi |
| worker process | `w3wp.exe` yang menjalankan app |
| virtual directory | mapping path URL ke folder |
| module | komponen pipeline request |
| handler | pemroses tipe request tertentu |
| log | access log IIS |

Alur request sederhana:

1. client konek ke IP/port.
2. HTTP.sys menerima request kernel mode.
3. IIS memilih site berdasarkan binding.
4. request diarahkan ke application pool.
5. worker process menjalankan aplikasi.
6. response dikirim ke client.
7. IIS menulis log.

```powershell
# Install IIS web server.
Install-WindowsFeature Web-Server -IncludeManagementTools

# Import module IIS administration klasik.
Import-Module WebAdministration

# Tampilkan IIS sites.
Get-Website

# Tampilkan application pools.
Get-ChildItem IIS:\AppPools

# Tampilkan worker process IIS.
Get-Process w3wp -ErrorAction SilentlyContinue

# Cek akses HTTP lokal.
Invoke-WebRequest http://localhost -UseBasicParsing
```

### 5.2 TLS Certificate, Binding, dan App Pool Identity

HTTPS di IIS membutuhkan certificate dan binding. Certificate harus punya private key, chain valid, subject/SAN cocok dengan hostname, belum expired, dan trusted oleh client.

Konsep TLS di IIS:

| Konsep | Arti |
|---|---|
| certificate store | lokasi certificate di Windows |
| private key | kunci rahasia untuk server TLS |
| binding HTTPS | mapping site ke certificate |
| SNI | memilih certificate berdasarkan hostname |
| cipher suite | algoritma TLS yang dinegosiasikan |
| chain | urutan certificate sampai root CA |

Application pool identity menentukan account yang menjalankan aplikasi. Jangan menjalankan app pool sebagai admin/domain admin. Gunakan identity minimum dan beri permission hanya pada folder/resource yang diperlukan.

```powershell
# Tampilkan certificate di LocalMachine My store.
Get-ChildItem Cert:\LocalMachine\My

# Tampilkan HTTPS binding IIS.
Get-WebBinding -Protocol https

# Tampilkan app pool identity.
Get-ItemProperty IIS:\AppPools\DefaultAppPool | Select-Object processModel

# Recycle application pool tertentu.
Restart-WebAppPool -Name "DefaultAppPool"

# Tampilkan log IIS default terbaru.
Get-ChildItem "C:\inetpub\logs\LogFiles" -Recurse -Filter "*.log" | Sort-Object LastWriteTime -Descending | Select-Object -First 5
```

### 5.3 Remote Desktop Services Overview

Remote Desktop Services memungkinkan user mengakses desktop/session/app dari server. RDS berbeda dari sekadar RDP admin. RDS adalah platform multi-user yang butuh licensing, broker, gateway, web access, dan security desain.

Komponen RDS:

| Komponen | Fungsi |
|---|---|
| RD Session Host | menjalankan session user |
| RD Connection Broker | mengarahkan user ke session/host |
| RD Gateway | akses RDP melalui HTTPS |
| RD Web Access | portal web app/desktop |
| RD Licensing | lisensi RDS CAL |
| User Profile Disk/FSLogix | profile user di environment RDS |

Security RDS:

- jangan expose RDP langsung ke internet
- gunakan VPN, RD Gateway, MFA, atau Zero Trust access
- batasi group yang boleh login RDP
- audit logon/logoff
- patch server dan aplikasi
- pisahkan admin RDP dan user RDS

```powershell
# Cek apakah Remote Desktop diaktifkan via registry.
Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Terminal Server" | Select-Object fDenyTSConnections

# Aktifkan Remote Desktop.
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections -Value 0

# Aktifkan firewall rule Remote Desktop.
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

# Tampilkan session user.
quser

# Tampilkan koneksi TCP ke port RDP.
Get-NetTCPConnection -LocalPort 3389 -ErrorAction SilentlyContinue
```

### 5.4 Print Server and Application Compatibility

Print Server masih banyak ditemukan di enterprise. Risiko utamanya adalah driver, permission, spooler security, dan compatibility. Print Spooler pernah menjadi permukaan serangan penting, jadi server yang tidak perlu printing sebaiknya tidak menjalankan service ini.

Konsep Print Server:

| Komponen | Fungsi |
|---|---|
| Print Spooler | service yang mengatur print job |
| driver | software printer |
| print queue | antrian printer |
| port | koneksi ke printer |
| permissions | siapa boleh print/manage |

Application compatibility di server perlu diuji karena aplikasi lama mungkin butuh .NET lama, VC++ runtime, COM component, path khusus, registry write, service account, atau permission folder.

```powershell
# Cek status Print Spooler.
Get-Service Spooler

# Stop Print Spooler jika server tidak membutuhkan printing.
Stop-Service Spooler

# Disable Print Spooler jika tidak dibutuhkan.
Set-Service Spooler -StartupType Disabled

# Tampilkan printer yang terdaftar.
Get-Printer

# Tampilkan driver printer.
Get-PrinterDriver
```

---

## 6.0 Virtualization and Containers

**Fokus teknis:** lihat VM sebagai compute, storage, network, checkpoint, dan isolation boundary.

```powershell
# Tampilkan VM Hyper-V.
Get-VM

# Tampilkan virtual switch.
Get-VMSwitch

# Tampilkan checkpoint VM.
Get-VMSnapshot -VMName *
```

Aspek teknis: VM state, vSwitch type, network mapping, disk path, checkpoint risk, dan dependency storage/network.

### 6.1 Hyper-V Architecture

Hyper-V adalah hypervisor type-1 Microsoft. Setelah role Hyper-V aktif, Windows berjalan sebagai parent partition yang mengelola child partitions atau VM. Hypervisor mengatur CPU scheduling, memory isolation, interrupt, dan akses device virtual.

Komponen Hyper-V:

| Komponen | Fungsi |
|---|---|
| parent partition | host management OS |
| child partition | VM |
| VMBus | channel komunikasi parent-child |
| VM worker process | process manajemen VM |
| VHDX | virtual disk |
| virtual switch | network switch virtual |
| integration services | driver/service guest untuk integrasi |
| checkpoint | point-in-time state untuk rollback lab |

- Hyper-V host sebaiknya tidak menjalankan role lain selain management/backup/monitoring
- jangan pakai checkpoint sebagai backup
- sizing CPU/RAM/storage harus berdasarkan workload
- time sync guest harus dipahami, terutama domain controller VM
- secure boot dan vTPM penting untuk guest modern

```powershell
# Install Hyper-V role dan management tools.
Install-WindowsFeature Hyper-V -IncludeManagementTools -Restart

# Tampilkan VM pada host.
Get-VM

# Tampilkan status Hyper-V host.
Get-VMHost

# Tampilkan virtual hard disk yang dipakai VM.
Get-VMHardDiskDrive -VMName "LAB-VM01"

# Tampilkan integration services VM.
Get-VMIntegrationService -VMName "LAB-VM01"
```

### 6.2 Virtual Switch, VLAN, dan VM Network

Virtual switch menentukan bagaimana VM terhubung ke network. Kesalahan virtual switch bisa membuat VM tidak bisa akses network, VLAN salah, atau host kehilangan koneksi management.

Jenis switch:

| Jenis | Fungsi |
|---|---|
| External | VM terhubung ke NIC fisik/network luar |
| Internal | VM dan host saling terhubung, tidak keluar host |
| Private | hanya antar VM di host yang sama |

Konsep:

| Konsep | Arti |
|---|---|
| VLAN access | VM masuk satu VLAN tertentu |
| trunk | membawa banyak VLAN ke VM tertentu |
| MAC spoofing | diperlukan untuk beberapa NVA/cluster scenario |
| SR-IOV | direct hardware path untuk performa |
| SET | Switch Embedded Teaming untuk teaming modern Hyper-V |

```powershell
# Tampilkan virtual switch.
Get-VMSwitch

# Buat external virtual switch memakai NIC fisik.
New-VMSwitch -Name "vSwitch-External" -NetAdapterName "Ethernet" -AllowManagementOS $true

# Set VLAN ID untuk adapter VM.
Set-VMNetworkAdapterVlan -VMName "LAB-VM01" -Access -VlanId 20

# Tampilkan adapter network VM.
Get-VMNetworkAdapter -VMName "LAB-VM01"

# Cek IP address yang dilaporkan integration service.
Get-VMNetworkAdapter -VMName "LAB-VM01" | Select-Object VMName, IPAddresses
```

### 6.3 VM Storage, Checkpoints, dan Integration Services

VHDX adalah format virtual disk modern Hyper-V. Disk bisa fixed, dynamic, atau differencing. Dynamic disk hemat space awal tetapi bisa fragmentasi dan butuh monitoring kapasitas. Fixed disk sering lebih predictable untuk workload penting.

Checkpoint:

| Jenis | Fungsi |
|---|---|
| Standard checkpoint | menyimpan state VM termasuk memory, cocok lab |
| Production checkpoint | memakai VSS/backup mechanism guest, lebih cocok workload |

Jangan biarkan checkpoint lama menumpuk. AVHDX chain panjang bisa menurunkan performa dan memperumit recovery.

Integration services:

| Service | Fungsi |
|---|---|
| Heartbeat | host tahu guest hidup |
| Time Synchronization | sinkron waktu host-guest |
| Data Exchange | metadata host-guest |
| Guest Service Interface | copy file host-guest |
| Backup VSS | backup VM-aware |

```powershell
# Buat VHDX dynamic baru.
New-VHD -Path "D:\VMs\LAB-VM01\disk1.vhdx" -SizeBytes 80GB -Dynamic

# Buat checkpoint production pada VM.
Checkpoint-VM -Name "LAB-VM01" -SnapshotName "BeforePatch"

# Tampilkan checkpoint VM.
Get-VMSnapshot -VMName "LAB-VM01"

# Hapus checkpoint setelah validasi.
Remove-VMSnapshot -VMName "LAB-VM01" -Name "BeforePatch"

# Tampilkan ukuran VHDX.
Get-VHD -Path "D:\VMs\LAB-VM01\disk1.vhdx"
```

### 6.4 Windows Containers Overview

Windows Containers menjalankan aplikasi terisolasi dengan container runtime. Ini berbeda dari VM. Container berbagi kernel dengan host container, sedangkan VM punya OS guest sendiri.

Jenis isolation:

| Jenis | Arti |
|---|---|
| Process isolation | container berbagi kernel host |
| Hyper-V isolation | container berjalan dalam utility VM untuk isolasi lebih kuat |

Konsep:

| Komponen | Fungsi |
|---|---|
| container image | template aplikasi dan dependency |
| container runtime | menjalankan container |
| base image | image dasar seperti Server Core atau Nano Server |
| registry | tempat menyimpan image |

Untuk security, perhatikan patch image, secret management, network exposure, dan user privilege di container.

```powershell
# Install Containers feature.
Install-WindowsFeature Containers

# Cek feature Containers.
Get-WindowsFeature Containers

# Tampilkan service terkait container jika runtime sudah dipasang.
Get-Service | Where-Object Name -match "docker|container"
```

---

## 7.0 High Availability and Scale

**Fokus teknis:** bedakan HA, backup, load balancing, dan disaster recovery.

```powershell
# Tampilkan cluster jika feature tersedia.
Get-Cluster

# Tampilkan cluster node jika cluster tersedia.
Get-ClusterNode

# Tampilkan resource cluster jika cluster tersedia.
Get-ClusterResource
```

Aspek teknis: quorum, node state, resource owner, failover behavior, dan bagian mana yang tetap single point of failure.

### 7.1 Failover Clustering Concepts

Failover Clustering membuat workload bisa pindah ke node lain saat node gagal atau maintenance. Cluster bukan solusi ajaib; cluster memperbaiki availability untuk failure tertentu, tetapi tidak menggantikan backup, monitoring, capacity planning, atau desain aplikasi yang benar.

Komponen cluster:

| Komponen | Fungsi |
|---|---|
| node | server anggota cluster |
| cluster name object | identitas cluster |
| resource | item yang dikelola cluster |
| role | workload yang dikelola cluster |
| heartbeat | komunikasi antar node |
| quorum | mekanisme voting agar cluster tidak split brain |
| witness | vote tambahan untuk quorum |
| CSV | Cluster Shared Volume untuk storage bersama |

Sebelum cluster dibuat:

- hardware/VM kompatibel
- network redundant
- DNS dan AD siap jika domain cluster
- storage sesuai workload
- driver/firmware konsisten
- cluster validation lulus
- time sync sehat

```powershell
# Install Failover Clustering feature.
Install-WindowsFeature Failover-Clustering -IncludeManagementTools

# Tampilkan command failover cluster.
Get-Command -Module FailoverClusters

# Jalankan validasi cluster untuk node kandidat.
Test-Cluster -Node LAB-HV-01, LAB-HV-02

# Buat cluster baru dengan static IP.
New-Cluster -Name LAB-CLU-01 -Node LAB-HV-01, LAB-HV-02 -StaticAddress 10.10.10.50

# Tampilkan node cluster.
Get-ClusterNode
```

### 7.2 Quorum, Witness, Cluster Network, dan CSV

Quorum menentukan apakah cluster punya cukup vote untuk tetap online. Tujuannya mencegah split brain, yaitu dua sisi cluster sama-sama merasa aktif dan menulis data yang sama.

Witness:

| Witness | Cocok untuk |
|---|---|
| File share witness | environment sederhana dengan file server stabil |
| Disk witness | cluster dengan shared disk |
| Cloud witness | environment dengan koneksi Azure |

Cluster network:

| Network | Fungsi |
|---|---|
| management | admin dan client access |
| cluster heartbeat | komunikasi node |
| live migration | pindah VM antar host |
| storage | iSCSI/SMB/S2D traffic |

CSV membuat volume cluster bisa diakses node secara serentak untuk workload seperti Hyper-V. CSV bukan folder share biasa; ia punya koordinasi cluster dan path seperti `C:\ClusterStorage\Volume1`.

```powershell
# Tampilkan konfigurasi quorum.
Get-ClusterQuorum

# Set file share witness.
Set-ClusterQuorum -FileShareWitness "\\LAB-FS-01\Witness"

# Tampilkan network cluster.
Get-ClusterNetwork

# Tampilkan Cluster Shared Volumes.
Get-ClusterSharedVolume

# Tampilkan resource cluster.
Get-ClusterResource
```

### 7.3 Clustered Workloads and Cluster Aware Updating

Workload cluster bisa berupa VM Hyper-V, file server, SQL Server FCI, DHCP, atau aplikasi lain yang cluster-aware. Setiap workload punya dependency berbeda.

Cluster Aware Updating atau CAU membantu patch node cluster bergiliran. CAU akan memindahkan workload dari node, patch/reboot node, lalu lanjut ke node berikutnya.

Pertanyaan sebelum membuat workload cluster:

| Pertanyaan | Alasan |
|---|---|
| workload stateful atau stateless | menentukan failover complexity |
| storage shared atau replicated | menentukan data consistency |
| client reconnect behavior | menentukan user impact |
| dependency DNS/certificate | menentukan failover name/IP |
| maintenance process | menentukan patching dan draining |

```powershell
# Tampilkan clustered roles.
Get-ClusterGroup

# Pindahkan clustered role ke node lain.
Move-ClusterGroup -Name "Virtual Machine LAB-VM01" -Node LAB-HV-02

# Tampilkan owner node sebuah resource.
Get-ClusterResource | Select-Object Name, OwnerGroup, OwnerNode, State

# Cek command Cluster Aware Updating.
Get-Command -Module ClusterAwareUpdating
```

### 7.4 Storage Spaces Direct

Storage Spaces Direct atau S2D membuat pool storage dari disk lokal beberapa node cluster. Ini sering dipakai untuk hyperconverged infrastructure, tempat compute dan storage berada di cluster yang sama.

Layer S2D:

| Layer | Fungsi |
|---|---|
| physical disks | NVMe/SSD/HDD lokal |
| storage pool | kumpulan disk antar node |
| virtual disk | disk virtual dari pool |
| volume | CSV/ReFS volume untuk workload |
| SMB3/RDMA | komunikasi storage antar node |

Hal penting:

- butuh hardware/network yang sesuai
- RDMA direkomendasikan untuk performa
- ReFS umum dipakai untuk volume S2D
- capacity planning harus memperhitungkan resilience
- monitor disk, network, latency, dan repair job
- S2D bukan pengganti backup

```powershell
# Cek disk fisik yang bisa dipool.
Get-PhysicalDisk | Sort-Object CanPool, MediaType

# Cek status cluster storage spaces direct.
Get-ClusterS2D

# Aktifkan Storage Spaces Direct pada cluster.
Enable-ClusterS2D

# Tampilkan storage pool.
Get-StoragePool

# Tampilkan virtual disk.
Get-VirtualDisk
```

### 7.5 Load Balancing and Resilience Patterns

High availability bisa dilakukan di banyak layer. Cluster bukan satu-satunya pola. Untuk web/app stateless, load balancing sering lebih sederhana daripada failover cluster.

Pola resilience:

| Pola | Cocok untuk |
|---|---|
| active/passive failover | workload stateful tradisional |
| active/active load balancing | web/app stateless |
| DNS failover | failover sederhana tapi bergantung TTL/cache |
| database replication | aplikasi dengan data stateful |
| queue-based design | workload asynchronous |
| backup restore | recovery dari data loss |
| geo DR | site failure |

Windows Network Load Balancing atau NLB tersedia, tetapi banyak environment modern memakai load balancer khusus, reverse proxy, cloud load balancer, atau ADC.

```powershell
# Install Network Load Balancing feature.
Install-WindowsFeature NLB -IncludeManagementTools

# Tampilkan command NLB.
Get-Command -Module NetworkLoadBalancingClusters

# Cek koneksi ke backend web dari server.
Test-NetConnection 10.10.10.81 -Port 443
```

---

## 8.0 Security and Hardening

**Fokus teknis:** validasi hardening sebagai konfigurasi yang bisa diverifikasi.

```powershell
# Tampilkan firewall profile.
Get-NetFirewallProfile

# Tampilkan local administrators.
Get-LocalGroupMember Administrators

# Tampilkan audit policy.
auditpol /get /category:*
```

Aspek teknis: admin lokal, inbound exposure, audit coverage, service yang tidak perlu, dan kontrol yang bisa memutus operasi jika terlalu agresif.

### 8.1 Security Baseline and Local Policy

Hardening server berarti mengurangi attack surface sambil menjaga service tetap berfungsi. Hardening yang baik harus berbasis baseline, diuji, dan punya rollback. Jangan mengaktifkan semua setting keras tanpa tahu dampaknya ke workload.

Area baseline:

| Area | Contoh |
|---|---|
| account policy | password, lockout |
| local rights | siapa boleh logon locally/RDP/service |
| audit policy | event yang dicatat |
| security options | UAC, anonymous access, SMB signing |
| firewall | inbound/outbound rule |
| services | disable service tidak perlu |
| protocols | disable legacy seperti SMB1/TLS lama |
| update | patch security rutin |

Local Security Policy (`secpol.msc`) dan Group Policy bisa mengatur banyak setting. Untuk server domain, GPO biasanya menjadi sumber policy utama. Untuk standalone server, local policy dan automation dipakai.

```powershell
# Export local security policy untuk review.
secedit /export /cfg C:\Temp\local-security-policy.inf

# Tampilkan audit policy saat ini.
auditpol /get /category:*

# Tampilkan local user rights assignment dari policy export.
Select-String -Path C:\Temp\local-security-policy.inf -Pattern "SeRemoteInteractiveLogonRight|SeServiceLogonRight|SeDebugPrivilege"

# Cek status SMB1 feature.
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol

# Disable SMB1 jika tidak dibutuhkan.
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart
```

### 8.2 Defender, Firewall, and Attack Surface Reduction

Microsoft Defender di Windows Server dapat memberi antivirus, real-time protection, cloud-delivered protection, dan integrasi dengan platform security lain tergantung environment. Windows Firewall harus tetap aktif dan dibuat spesifik sesuai role.

Firewall profile:

| Profile | Arti |
|---|---|
| Domain | ketika server mengenali domain |
| Private | network terpercaya non-domain |
| Public | network tidak terpercaya |

Rule firewall harus didesain berdasarkan service:

| Role | Inbound umum |
|---|---|
| DNS Server | TCP/UDP 53 dari subnet/client tertentu |
| DHCP Server | UDP 67/68 sesuai network |
| File Server | TCP 445 dari client subnet |
| IIS | TCP 80/443 dari client/load balancer |
| WinRM | TCP 5985/5986 dari admin subnet |
| RDP | TCP/UDP 3389 dari jump host/admin subnet |

```powershell
# Cek status Defender.
Get-MpComputerStatus

# Tampilkan preference Defender.
Get-MpPreference

# Update signature Defender.
Update-MpSignature

# Tampilkan firewall profile.
Get-NetFirewallProfile

# Tampilkan inbound rule aktif.
Get-NetFirewallRule -Direction Inbound -Enabled True | Select-Object DisplayName, Profile, Action

# Buat rule WinRM hanya dari subnet admin.
New-NetFirewallRule -DisplayName "Allow WinRM from Admin Subnet" -Direction Inbound -Protocol TCP -LocalPort 5985 -RemoteAddress 10.10.99.0/24 -Action Allow
```

### 8.3 Service Accounts, Privilege, and Secrets

Service account adalah identity untuk menjalankan service atau scheduled task. Risiko utamanya adalah privilege berlebihan, password statis, reuse password, interactive logon, dan secret yang tersebar di script/config.

Jenis account:

| Jenis | Kegunaan |
|---|---|
| LocalSystem | privilege sangat tinggi di lokal, identity komputer di network |
| LocalService | privilege rendah lokal, anonymous/network terbatas |
| NetworkService | privilege rendah lokal, identity komputer di network |
| local user | service lokal non-domain |
| domain user | service lintas server, perlu password management |
| gMSA | managed service account domain dengan password dikelola AD |

Prinsip:

- jangan pakai domain admin untuk service
- beri `Log on as a service` hanya pada account yang perlu
- deny interactive logon untuk service account jika sesuai
- rotasi secret
- audit event logon service account
- gunakan gMSA untuk service domain-aware jika memungkinkan

```powershell
# Tampilkan service beserta account yang dipakai.
Get-CimInstance Win32_Service | Select-Object Name, StartName, State | Sort-Object StartName

# Cari service yang berjalan dengan domain account.
Get-CimInstance Win32_Service | Where-Object StartName -like "*\\*" | Select-Object Name, StartName, State

# Tampilkan scheduled task dan principal.
Get-ScheduledTask | Select-Object TaskName, TaskPath, State

# Tampilkan detail principal scheduled task tertentu.
Get-ScheduledTask -TaskName "MyTask" | Select-Object -ExpandProperty Principal
```

### 8.4 Credential Protection and Remote Admin Security

Admin server harus menghindari penyebaran credential. Saat admin login interaktif ke banyak server, credential bisa tertinggal di memory/session tertentu. Gunakan jump host, tiering admin, PowerShell remoting, JEA, dan credential hygiene.

Konsep:

| Konsep | Arti |
|---|---|
| LSASS | process yang memegang material authentication |
| Credential Guard | proteksi credential berbasis virtualization security |
| Protected Users | group AD dengan pembatasan auth kuat |
| Restricted Admin RDP | mode RDP yang mengurangi exposure credential |
| JEA | admin dibatasi pada command tertentu |
| PAW | Privileged Access Workstation |

Remote admin best practice:

- admin dari workstation khusus, bukan endpoint harian
- batasi RDP ke jump host/admin subnet
- gunakan MFA untuk remote access
- jangan browse internet dari server
- jangan simpan password di script plain text
- gunakan Just Enough Administration untuk task operasional

```powershell
# Cek apakah LSA protection registry aktif.
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" | Select-Object RunAsPPL

# Cek group local Administrators.
Get-LocalGroupMember Administrators

# Tampilkan session logon aktif.
query user

# Tampilkan listener WinRM.
winrm enumerate winrm/config/listener

# Cek setting RDP terkait NLA.
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" | Select-Object UserAuthentication
```

### 8.5 Logging, Auditing, and Detection

Server harus menghasilkan log yang cukup untuk troubleshooting dan detection. Audit terlalu sedikit membuat incident gelap. Audit terlalu banyak tanpa pipeline membuat noise dan storage penuh.

Log penting:

| Log | Isi |
|---|---|
| System | service, driver, boot, hardware |
| Application | aplikasi dan role tertentu |
| Security | logon, privilege, object access, policy |
| Setup | install/update |
| PowerShell Operational | script block/module logging |
| Windows Defender Operational | detection dan config |
| DNS Server | DNS service |
| DHCP Server audit | lease dan DHCP event |
| SMBServer Operational | SMB operational events |
| FailoverClustering | cluster |

Event ID penting:

| Event ID | Arti |
|---:|---|
| 4624 | logon sukses |
| 4625 | logon gagal |
| 4648 | logon dengan explicit credentials |
| 4672 | special privileges assigned |
| 4688 | process creation jika audit aktif |
| 4697 | service installed |
| 4698 | scheduled task created |
| 4720 | user account created |
| 4732 | member added to local group |
| 7045 | service installed di System log |
| 1102 | audit log cleared |
| 4103 | PowerShell module logging |
| 4104 | PowerShell script block logging |

```powershell
# Tampilkan log yang tersedia.
Get-WinEvent -ListLog * | Sort-Object LogName | Select-Object LogName, RecordCount, IsEnabled

# Ambil event logon gagal terbaru.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625} -MaxEvents 20

# Ambil event service installed dari System log.
Get-WinEvent -FilterHashtable @{LogName="System"; Id=7045} -MaxEvents 20

# Cek PowerShell Operational log.
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" -MaxEvents 20

# Perbesar ukuran Security log.
wevtutil sl Security /ms:1073741824
```

---

## 9.0 Monitoring and Troubleshooting

**Fokus teknis:** gunakan log dan counter untuk menunjukkan health server.

```powershell
# Ambil error System log terbaru.
Get-WinEvent -FilterHashtable @{LogName='System'; Level=2} -MaxEvents 30

# Melihat counter CPU dan memory.
Get-Counter "\Processor(_Total)\% Processor Time","\Memory\Available MBytes"

# Test koneksi ke service SMB.
Test-NetConnection server.lab.local -Port 445
```

Aspek teknis: event ID, timestamp, resource pressure, service impact, dan command validasi setelah recovery.

### 9.1 Event Logs, Reliability, and Data Teknis

Troubleshooting server harus dimulai dari data. Jangan langsung reboot kecuali service impact membutuhkan tindakan cepat dan data sudah cukup atau bisa dikumpulkan.

Urutan data teknis:

1. scope masalah
2. waktu mulai
3. user/client terdampak
4. service dan port terdampak
5. event log sekitar waktu masalah
6. perubahan terakhir
7. resource CPU/RAM/disk/network
8. dependency seperti DNS, AD, storage, certificate

Event Viewer bagus untuk eksplorasi GUI, tetapi `Get-WinEvent` lebih kuat untuk filter, export, dan automation.

```powershell
# Ambil event critical dan error dari System dalam 24 jam terakhir.
Get-WinEvent -FilterHashtable @{LogName="System"; Level=1,2; StartTime=(Get-Date).AddDays(-1)}

# Ambil event error Application dalam 24 jam terakhir.
Get-WinEvent -FilterHashtable @{LogName="Application"; Level=2; StartTime=(Get-Date).AddDays(-1)}

# Export System log untuk analisis.
wevtutil epl System C:\Temp\System.evtx

# Tampilkan reliability records terbaru.
Get-CimInstance Win32_ReliabilityRecords | Select-Object -First 20 TimeGenerated, SourceName, Message
```

### 9.2 Performance Monitor and Counters

Performance troubleshooting harus membedakan bottleneck CPU, memory, disk, network, lock/contention, dan dependency eksternal. Satu angka seperti CPU 90% belum cukup; lihat queue, latency, throughput, dan process penyebab.

Counter penting:

| Area | Counter |
|---|---|
| CPU | `Processor(_Total)\% Processor Time` |
| CPU queue | `System\Processor Queue Length` |
| Memory | `Memory\Available MBytes` |
| Paging | `Memory\Pages/sec` |
| Disk latency | `PhysicalDisk(*)\Avg. Disk sec/Read` dan Write |
| Disk queue | `PhysicalDisk(*)\Current Disk Queue Length` |
| Network | `Network Interface(*)\Bytes Total/sec` |
| SMB | `SMB Server Shares(*)\Avg. sec/Read` |

Interpretasi umum:

| Gejala | Kemungkinan |
|---|---|
| CPU tinggi dan queue tinggi | CPU bottleneck |
| Available memory rendah dan paging tinggi | memory pressure |
| disk latency tinggi | storage bottleneck |
| network throughput tinggi dan retransmit | network congestion/packet loss |
| satu process dominan | app/service issue |

```powershell
# Tampilkan process paling tinggi CPU kumulatif.
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, Id, CPU, WorkingSet

# Ambil counter CPU dan memory.
Get-Counter "\Processor(_Total)\% Processor Time","\Memory\Available MBytes"

# Ambil counter disk latency.
Get-Counter "\PhysicalDisk(*)\Avg. Disk sec/Read","\PhysicalDisk(*)\Avg. Disk sec/Write"

# Tampilkan top process berdasarkan working set.
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10 Name, Id, WorkingSet

# Buka Performance Monitor GUI.
perfmon
```

### 9.3 Network Troubleshooting

Network troubleshooting server harus memisahkan DNS resolution, routing, firewall, service listen, authentication, dan application response.

Urutan umum:

1. IP config benar.
2. Route ke target ada.
3. DNS resolve ke IP yang benar.
4. Server listen di port.
5. Firewall mengizinkan traffic.
6. TCP handshake berhasil.
7. Authentication/authorization berhasil.
8. Aplikasi memberi response benar.

```powershell
# Cek IP config server.
Get-NetIPConfiguration

# Cek DNS resolution nama target.
Resolve-DnsName fs01.lab.local

# Test koneksi TCP ke port target.
Test-NetConnection fs01.lab.local -Port 445

# Tampilkan port listening.
Get-NetTCPConnection -State Listen | Sort-Object LocalPort

# Tampilkan process pemilik port tertentu.
Get-Process -Id (Get-NetTCPConnection -LocalPort 445 -State Listen).OwningProcess

# Trace route ke target.
Test-NetConnection fs01.lab.local -TraceRoute
```

### 9.4 Storage and File Server Troubleshooting

Masalah file server bisa berasal dari disk penuh, ACL, share permission, SMB session, file lock, antivirus scan, network latency, DFS referral, atau storage backend.

Gejala umum:

| Gejala | Cek |
|---|---|
| access denied | effective permission, group membership, share + NTFS ACL |
| file locked | SMB open files, process lokal |
| lambat buka folder | jumlah file, antivirus, network, SMB signing/encryption |
| path tidak ditemukan | DFS referral, DNS, share name |
| disk penuh | quota, shadow copies, log, user data |
| backup gagal | VSS writer, storage free space |

```powershell
# Cek free space volume.
Get-Volume | Select-Object DriveLetter, FileSystemLabel, SizeRemaining, Size

# Tampilkan SMB open files.
Get-SmbOpenFile

# Tutup file SMB tertentu berdasarkan FileId.
Close-SmbOpenFile -FileId 123456789 -Force

# Cek permission folder.
icacls "D:\Shares\Finance"

# Cari file terbesar di folder share.
Get-ChildItem "D:\Shares" -Recurse -File | Sort-Object Length -Descending | Select-Object -First 20 FullName, Length
```

### 9.5 Boot, Update, and Component Store Repair

Masalah boot/update sering berkaitan dengan driver, boot configuration, component store, pending update, disk corruption, atau service dependency. Jangan menghapus folder sistem seperti `WinSxS` secara manual.

Komponen:

| Komponen | Fungsi |
|---|---|
| BCD | boot configuration data |
| WinSxS | component store |
| DISM | repair image/component store |
| SFC | scan file sistem protected |
| CBS log | Component Based Servicing log |
| Safe Mode | boot minimal untuk recovery |

Urutan repair umum:

1. cek event Setup/System
2. cek pending reboot
3. cek disk health
4. jalankan DISM restore health
5. jalankan SFC
6. review CBS log
7. rollback update/driver jika terlihat sebagai penyebab

```powershell
# Cek health component store.
DISM /Online /Cleanup-Image /ScanHealth

# Repair component store online.
DISM /Online /Cleanup-Image /RestoreHealth

# Scan protected system files.
sfc /scannow

# Cek disk filesystem.
chkdsk C: /scan

# Tampilkan boot configuration.
bcdedit /enum
```

---

## 10.0 Active Directory Bridge

**Fokus teknis:** verifikasi kapan masalah server sebenarnya masalah identity/DNS/domain.

```powershell
# Query domain DNS.
Resolve-DnsName lab.local

# Query SRV record domain controller.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Test secure channel domain.
Test-ComputerSecureChannel
```

Aspek teknis: DNS resolver, DC locator, secure channel state, time sync, dan apakah server memakai identity domain dengan benar.

### 10.1 Member Server vs Domain Controller

Member server adalah Windows Server yang join domain tetapi bukan domain controller. Domain controller menjalankan AD DS dan menyimpan directory database. Jangan mencampur peran tanpa alasan kuat.

Perbedaan:

| Area | Member Server | Domain Controller |
|---|---|---|
| Local users | punya local SAM | tidak punya local user biasa seperti member |
| Authentication role | memakai domain untuk auth | melayani auth domain |
| Workload | file, app, IIS, Hyper-V, RDS | AD DS, DNS AD-integrated |
| Security | bisa dikelola seperti server role | tier tertinggi, sangat sensitif |
| Restore | restore workload | restore AD perlu prosedur khusus |

Prinsip:

- domain controller tidak dipakai sebagai file/app server umum
- admin domain tidak dipakai untuk login ke member server sembarangan
- member server kritis punya OU dan GPO khusus
- server role dikelompokkan berdasarkan tier dan sensitivity

```powershell
# Cek apakah server join domain.
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain

# Cek role domain controller dari feature AD DS.
Get-WindowsFeature AD-Domain-Services

# Tampilkan domain yang terlihat oleh server.
whoami /fqdn
```

### 10.2 Domain Join and DNS Dependency

Domain join bergantung pada DNS. Client harus bisa resolve domain dan menemukan domain controller melalui SRV record. Jika DNS client server mengarah ke DNS publik, domain join biasanya gagal atau lambat.

Syarat domain join:

| Syarat | Detail |
|---|---|
| DNS | mengarah ke DNS internal domain |
| Time | skew tidak berlebihan |
| Network | bisa reach DC port penting |
| Credential | account punya hak join computer |
| Computer object | belum konflik atau boleh reuse |

```powershell
# Set DNS client ke DNS internal sebelum domain join.
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.10.10.10

# Test resolve domain.
Resolve-DnsName lab.local

# Test SRV record domain controller.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Join server ke domain dan restart.
Add-Computer -DomainName lab.local -Restart
```

### 10.3 Group Policy Impact on Servers

GPO bisa mengubah firewall, user rights, audit policy, services, registry, scripts, scheduled tasks, certificate trust, Windows Update, Defender, dan banyak konfigurasi lain. Saat troubleshooting server domain, selalu cek GPO yang berlaku.

Pertanyaan penting:

| Pertanyaan | Alasan |
|---|---|
| server ada di OU mana | menentukan GPO scope |
| security filtering apa | menentukan apakah GPO apply |
| WMI filter ada atau tidak | bisa mengecualikan server |
| kapan policy terakhir update | menentukan timing perubahan |
| setting konflik di mana | GPO precedence |

```powershell
# Update Group Policy pada server.
gpupdate /force

# Tampilkan Resultant Set of Policy ringkas.
gpresult /r

# Export report GPO applied ke HTML.
gpresult /h C:\Temp\gpresult.html

# Tampilkan policy audit efektif.
auditpol /get /category:*
```

### 10.4 What Moves to Active-Directory.md

Bagian berikut sebaiknya tidak diperdalam di file ini agar Windows Server tetap fokus:

| Topik | Alasan dipindah |
|---|---|
| Forest, tree, domain | konsep inti AD |
| Domain controller promotion | lifecycle AD DS |
| Kerberos detail | authentication domain |
| LDAP query | directory operations |
| FSMO roles | AD operations |
| Sites and Services | replication topology |
| SYSVOL and DFSR | GPO/logon scripts replication |
| Group Policy design | policy domain |
| AD CS | PKI enterprise |
| AD attack paths | security engineering AD |
| Tiering model | privilege architecture |
| Delegation | least privilege AD |

Setelah file ini matang, lanjut paling logis adalah `Active-Directory.md`.

---
