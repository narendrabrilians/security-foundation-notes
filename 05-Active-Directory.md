# Active Directory

Catatan Active Directory pribadi untuk belajar serius, praktik lab, troubleshooting, dan membangun fondasi identity berbasis domain.

Fokus halaman ini adalah Active Directory Domain Services sebagai identity backbone: domain, forest, domain controller, DNS, Kerberos, LDAP, OU, group, GPO, replication, delegation, backup/restore, dan security operations.

Referensi resmi yang berguna:

- Active Directory Domain Services overview: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview
- AD DS design and planning: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/ad-ds-design-and-planning
- Install or remove AD DS: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-
- Kerberos authentication overview: https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview
- Group Policy overview: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview
- AD DS replication concepts: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/replication/active-directory-replication-concepts
- Operations master roles: https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles
- AD DS sites: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-domain-services-sites
- Securing privileged access: https://learn.microsoft.com/en-us/security/privileged-access-workstations/privileged-access-access-model
- Security groups: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups
- AD Recycle Bin: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/active-directory-recycle-bin
- Backup and restore domain controllers: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/ad-forest-recovery-backup

Area utama:

| Area | Cakupan |
|---|---|
| Fundamentals | domain, forest, tree, OU, object, schema, global catalog |
| Domain Controllers | promotion, demotion, DNS dependency, SYSVOL, health check |
| Authentication | Kerberos, NTLM, LDAP, secure channel, tickets, SPN |
| Administration | users, groups, computers, OU, delegation, PowerShell |
| Group Policy | GPO processing, link, inheritance, security filtering, loopback |
| Replication | sites, subnets, KCC, connection objects, SYSVOL, DFSR |
| Operations | FSMO, backup, restore, Recycle Bin, monitoring, runbook |
| Security | tiering, admin model, hardening DC, audit, attack path awareness |
| Troubleshooting | DNS, logon, replication, GPO, lockout, time, secure channel |

## Daftar Isi

- [0. Cara Pakai Catatan Ini](#0-cara-pakai-catatan-ini)
- [1.0 AD DS Fundamentals](#10-ad-ds-fundamentals)
  - [1.1 Apa Itu Active Directory](#11-apa-itu-active-directory)
  - [1.2 Forest, Tree, Domain, dan Trust](#12-forest-tree-domain-dan-trust)
  - [1.3 Object, Attribute, Schema, dan Distinguished Name](#13-object-attribute-schema-dan-distinguished-name)
  - [1.4 OU, Container, dan Administrative Boundary](#14-ou-container-dan-administrative-boundary)
  - [1.5 Global Catalog, UPN, SID, dan RID](#15-global-catalog-upn-sid-dan-rid)
- [2.0 Domain Controller and DNS](#20-domain-controller-and-dns)
  - [2.1 Domain Controller Role](#21-domain-controller-role)
  - [2.2 AD-Integrated DNS and SRV Records](#22-ad-integrated-dns-and-srv-records)
  - [2.3 Promoting and Demoting Domain Controllers](#23-promoting-and-demoting-domain-controllers)
  - [2.4 SYSVOL, NETLOGON, and DFSR](#24-sysvol-netlogon-and-dfsr)
  - [2.5 Domain Controller Health Checks](#25-domain-controller-health-checks)
- [3.0 Identity Objects](#30-identity-objects)
  - [3.1 Users](#31-users)
  - [3.2 Groups](#32-groups)
  - [3.3 Computers and Secure Channel](#33-computers-and-secure-channel)
  - [3.4 Service Accounts, SPN, and gMSA](#34-service-accounts-spn-and-gmsa)
  - [3.5 Contacts, Disabled Objects, and Lifecycle](#35-contacts-disabled-objects-and-lifecycle)
- [4.0 Authentication and Authorization](#40-authentication-and-authorization)
  - [4.1 Kerberos Flow](#41-kerberos-flow)
  - [4.2 NTLM and Legacy Authentication](#42-ntlm-and-legacy-authentication)
  - [4.3 LDAP, LDAPS, and Directory Query](#43-ldap-ldaps-and-directory-query)
  - [4.4 Access Token, SID, Group Membership, and ACL](#44-access-token-sid-group-membership-and-acl)
  - [4.5 Time, Password Policy, and Account Lockout](#45-time-password-policy-and-account-lockout)
- [5.0 Group Policy](#50-group-policy)
  - [5.1 GPO Components and Processing](#51-gpo-components-and-processing)
  - [5.2 Link Order, Inheritance, Enforced, and Block Inheritance](#52-link-order-inheritance-enforced-and-block-inheritance)
  - [5.3 Security Filtering, WMI Filtering, and Item-Level Targeting](#53-security-filtering-wmi-filtering-and-item-level-targeting)
  - [5.4 Loopback Processing](#54-loopback-processing)
  - [5.5 GPO Troubleshooting](#55-gpo-troubleshooting)
- [6.0 Replication, Sites, and FSMO](#60-replication-sites-and-fsmo)
  - [6.1 AD Replication Concepts](#61-ad-replication-concepts)
  - [6.2 Sites, Subnets, Site Links, and KCC](#62-sites-subnets-site-links-and-kcc)
  - [6.3 Repadmin and Replication Troubleshooting](#63-repadmin-and-replication-troubleshooting)
  - [6.4 FSMO Roles](#64-fsmo-roles)
  - [6.5 RID Pool, USN, Tombstone, and Lingering Objects](#65-rid-pool-usn-tombstone-and-lingering-objects)
- [7.0 Administration and Delegation](#70-administration-and-delegation)
  - [7.1 Administrative Tools](#71-administrative-tools)
  - [7.2 OU Design](#72-ou-design)
  - [7.3 Delegation Model](#73-delegation-model)
  - [7.4 Bulk Administration with PowerShell](#74-bulk-administration-with-powershell)
  - [7.5 Change Control and Documentation](#75-change-control-and-documentation)
- [8.0 Security Engineering](#80-security-engineering)
  - [8.1 Tiering Model and Privileged Access](#81-tiering-model-and-privileged-access)
  - [8.2 Domain Controller Hardening](#82-domain-controller-hardening)
  - [8.3 Admin Groups and Dangerous Rights](#83-admin-groups-and-dangerous-rights)
  - [8.4 Kerberos Security, Delegation, and SPN Risk](#84-kerberos-security-delegation-and-spn-risk)
  - [8.5 Audit Policy, Event IDs, and Detection](#85-audit-policy-event-ids-and-detection)
  - [8.6 Common AD Attack Paths as Defensive Knowledge](#86-common-ad-attack-paths-as-defensive-knowledge)
- [9.0 Backup, Restore, and Recovery](#90-backup-restore-and-recovery)
  - [9.1 System State Backup](#91-system-state-backup)
  - [9.2 AD Recycle Bin and Object Restore](#92-ad-recycle-bin-and-object-restore)
  - [9.3 Authoritative and Non-Authoritative Restore](#93-authoritative-and-non-authoritative-restore)
  - [9.4 Forest Recovery Overview](#94-forest-recovery-overview)
  - [9.5 Disaster Recovery Runbook](#95-disaster-recovery-runbook)
- [10.0 Troubleshooting Playbooks](#100-troubleshooting-playbooks)
  - [10.1 User Cannot Log In](#101-user-cannot-log-in)
  - [10.2 Account Lockout](#102-account-lockout)
  - [10.3 Domain Join Fails](#103-domain-join-fails)
  - [10.4 GPO Not Applying](#104-gpo-not-applying)
  - [10.5 Replication Broken](#105-replication-broken)
  - [10.6 DNS and DC Locator Failure](#106-dns-and-dc-locator-failure)
- [Lab Checklist](#lab-checklist)
- [Command Index Cepat](#command-index-cepat)
- [Checklist Kesiapan Praktik](#checklist-kesiapan-praktik)

---

## 0. Cara Pakai Catatan Ini

Active Directory tidak boleh dipahami hanya sebagai tempat membuat user. AD adalah sistem identity terdistribusi yang menjadi dasar login, authorization, policy, DNS internal, service authentication, audit, dan privilege management.

Saat belajar atau troubleshooting AD, biasakan bertanya:

- object apa yang bermasalah: user, computer, group, GPO, OU, DC, DNS record, SPN, atau trust
- domain controller mana yang dipakai client saat masalah terjadi
- apakah DNS client mengarah ke DNS internal yang benar
- apakah waktu client, server, dan DC sinkron
- apakah masalah authentication, authorization, replication, policy, atau network
- event log mana yang membuktikan kegagalan
- apakah perubahan sudah replicate ke semua DC
- apakah privilege admin yang dipakai sesuai tier

Format command:

```powershell
# Setiap command diberi komentar singkat.
Get-Command
```

Lab minimum:

| Komponen | Rekomendasi |
|---|---|
| Domain controller | 2 VM Windows Server |
| Member server | 1 VM Windows Server |
| Client | 1 VM Windows 10/11 |
| Domain | `lab.local` atau domain lab lain |
| DNS | AD-integrated DNS pada DC |
| Snapshot | sebelum promote DC, ubah GPO besar, dan simulasi failure |
| Tools | ADUC, ADAC, DNS Manager, GPMC, Sites and Services, PowerShell |

Catatan penting:

- Jangan memakai domain produksi untuk eksperimen.
- Jangan menjalankan workload umum di domain controller.
- Jangan login ke endpoint biasa memakai Domain Admin.
- Jangan menghapus object AD sebelum paham dependency dan restore path.
- Jangan mengubah DNS AD tanpa tahu dampak ke DC locator.

---

## 1.0 AD DS Fundamentals

### 1.1 Apa Itu Active Directory

Active Directory Domain Services atau AD DS adalah directory service yang menyimpan identity dan configuration object. Object itu bisa berupa user, group, computer, printer, OU, GPO, service connection point, DNS record tertentu, dan banyak object lain.

AD menjawab pertanyaan seperti:

| Pertanyaan | Jawaban AD |
|---|---|
| Siapa user ini | user object, SID, UPN, attribute |
| Apakah password benar | authentication melalui DC |
| User ini anggota group apa | group membership dan token |
| Komputer ini bagian domain mana | computer object dan secure channel |
| Policy apa yang berlaku | GPO berdasarkan site, domain, OU |
| Di mana domain controller | DNS SRV record |
| Service ini berjalan sebagai siapa | service account dan SPN |

AD DS bukan LDAP saja. LDAP adalah protocol query/modify directory. AD DS juga memakai Kerberos, DNS, SMB/SYSVOL, RPC, replication engine, Group Policy, dan security descriptor.

Komponen besar AD:

| Komponen | Fungsi |
|---|---|
| Domain | boundary administrasi dan replication utama |
| Forest | security boundary tertinggi AD |
| Domain Controller | server yang menyimpan dan melayani directory |
| DNS | dependency name resolution dan DC locator |
| Kerberos | authentication utama domain modern |
| LDAP | query dan update directory |
| Group Policy | konfigurasi user/computer terpusat |
| SYSVOL | file policy dan script yang dibagikan DC |
| Replication | sinkronisasi data antar DC |

```powershell
# Import module Active Directory.
Import-Module ActiveDirectory

# Tampilkan informasi domain saat ini.
Get-ADDomain

# Tampilkan informasi forest saat ini.
Get-ADForest

# Tampilkan domain controller yang diketahui domain.
Get-ADDomainController -Filter *
```

### 1.2 Forest, Tree, Domain, dan Trust

Forest adalah security boundary tertinggi di AD. Semua domain dalam satu forest berbagi schema, configuration partition, dan global catalog. Jika satu domain dalam forest benar-benar compromise pada level enterprise admin atau equivalent, seluruh forest harus dianggap berisiko.

Domain adalah boundary administrasi, replication, password policy default, dan namespace. Domain bukan security boundary absolut terhadap admin forest/domain lain.

Tree adalah kumpulan domain dengan namespace berurutan, misalnya `corp.example.com` dan `id.corp.example.com`. Forest bisa berisi lebih dari satu tree.

Trust memungkinkan principal dari satu domain/forest diakui oleh domain/forest lain.

| Konsep | Arti |
|---|---|
| Forest | security boundary utama |
| Domain | administrative dan replication boundary |
| Tree | domain hierarchy dengan namespace contiguous |
| Child domain | domain di bawah parent namespace |
| Trust | relasi kepercayaan authentication antar domain/forest |
| Transitive trust | trust berlaku melewati domain lain |
| One-way trust | satu arah percaya |
| Two-way trust | dua arah percaya |

Contoh:

| Nama | Jenis |
|---|---|
| `corp.local` | forest root domain |
| `id.corp.local` | child domain |
| `corp.example.com` dan `research.example.net` | dua tree dalam satu forest jika dibuat begitu |
| trust dengan partner | external/forest trust tergantung desain |

Desain modern biasanya menghindari banyak domain kecuali ada alasan kuat seperti regulatory boundary, namespace legacy, atau administrative separation yang jelas. Terlalu banyak domain menambah kompleksitas trust, replication, DNS, GPO, dan operasi.

```powershell
# Tampilkan nama domain dan parent domain.
Get-ADDomain | Select-Object DNSRoot, NetBIOSName, ParentDomain, Forest

# Tampilkan daftar domain dalam forest.
(Get-ADForest).Domains

# Tampilkan trust yang terkonfigurasi.
Get-ADTrust -Filter *

# Tampilkan functional level domain dan forest.
Get-ADDomain | Select-Object DomainMode

# Tampilkan functional level forest.
Get-ADForest | Select-Object ForestMode
```

### 1.3 Object, Attribute, Schema, dan Distinguished Name

AD menyimpan data sebagai object. Setiap object punya attribute. Schema mendefinisikan jenis object dan attribute apa yang valid.

Contoh user object punya attribute seperti:

| Attribute | Contoh | Arti |
|---|---|---|
| `sAMAccountName` | `alice` | logon name legacy |
| `userPrincipalName` | `alice@lab.local` | UPN logon modern |
| `displayName` | `Alice Tan` | nama tampilan |
| `objectSid` | `S-1-5-21-...` | SID unik security principal |
| `memberOf` | group DN | group langsung |
| `pwdLastSet` | timestamp | kapan password terakhir diganti |
| `userAccountControl` | bit flags | status account dan opsi login |

Distinguished Name atau DN adalah path unik object dalam directory.

Contoh DN:

```text
CN=Alice Tan,OU=Users,OU=Jakarta,DC=lab,DC=local
```

Bagian DN:

| Bagian | Arti |
|---|---|
| `CN=Alice Tan` | common name object |
| `OU=Users` | organizational unit |
| `OU=Jakarta` | parent OU |
| `DC=lab,DC=local` | domain component `lab.local` |

Naming attribute penting:

| Attribute | Fungsi |
|---|---|
| `distinguishedName` | path LDAP lengkap |
| `canonicalName` | path lebih mudah dibaca |
| `objectGUID` | identifier unik object, stabil walau object dipindah |
| `objectSid` | security identifier untuk permission |
| `sAMAccountName` | nama logon pre-Windows 2000 |
| `userPrincipalName` | nama logon format email |

Schema harus diperlakukan hati-hati. Perubahan schema bersifat forest-wide dan tidak bisa dianggap seperti setting biasa.

```powershell
# Ambil user beserta attribute penting.
Get-ADUser alice -Properties displayName, userPrincipalName, memberOf, pwdLastSet, userAccountControl

# Tampilkan distinguished name user.
Get-ADUser alice | Select-Object DistinguishedName

# Cari object berdasarkan LDAP filter.
Get-ADObject -LDAPFilter "(sAMAccountName=alice)" -Properties *

# Tampilkan schema master FSMO holder.
Get-ADForest | Select-Object SchemaMaster
```

### 1.4 OU, Container, dan Administrative Boundary

Organizational Unit atau OU dipakai untuk mengatur object, menerapkan GPO, dan mendelegasikan administrasi. OU bukan security boundary kuat. OU membantu manajemen, tetapi admin dengan privilege tinggi tetap bisa mengubah banyak hal.

Container mirip folder AD, tetapi tidak semua container bisa langsung dipakai untuk link GPO seperti OU. Contoh default container adalah `CN=Users` dan `CN=Computers`.

Perbedaan:

| Item | Bisa link GPO | Cocok untuk |
|---|---|---|
| OU | ya | desain administrasi dan policy |
| Container | tidak seperti OU | object default/bawaan |
| Domain root | ya | policy domain umum |
| Site | ya | policy berdasarkan lokasi/site |

Desain OU yang baik biasanya berdasarkan:

- role object, bukan hanya struktur organisasi HR
- kebutuhan GPO
- kebutuhan delegation
- tier security
- lifecycle object
- server role dan sensitivity

Contoh OU:

```text
DC=lab,DC=local
├── OU=Admin
│   ├── OU=Tier0
│   ├── OU=Tier1
│   └── OU=Tier2
├── OU=Servers
│   ├── OU=Domain Controllers
│   ├── OU=File Servers
│   └── OU=Web Servers
├── OU=Workstations
├── OU=Users
└── OU=Groups
```

```powershell
# Buat OU Servers di root domain.
New-ADOrganizationalUnit -Name "Servers" -Path "DC=lab,DC=local"

# Buat child OU File Servers.
New-ADOrganizationalUnit -Name "File Servers" -Path "OU=Servers,DC=lab,DC=local"

# Pindahkan computer object ke OU File Servers.
Move-ADObject -Identity "CN=LAB-FS-01,CN=Computers,DC=lab,DC=local" -TargetPath "OU=File Servers,OU=Servers,DC=lab,DC=local"

# Tampilkan OU yang ada.
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName
```

### 1.5 Global Catalog, UPN, SID, dan RID

Global Catalog atau GC menyimpan partial replica dari semua object di forest. GC penting untuk logon, universal group membership, address book, dan query forest-wide.

UPN adalah logon name seperti email, misalnya `alice@lab.local`. UPN suffix bisa berbeda dari DNS domain jika dikonfigurasi, misalnya user login dengan `alice@company.com` walau domain internal `corp.local`.

SID adalah Security Identifier. Permission Windows mengacu ke SID, bukan nama yang terlihat. Karena itu rename user tidak menghapus permission karena SID tetap sama.

RID adalah Relative Identifier, bagian akhir SID yang diberikan oleh RID Master. Contoh:

```text
Domain SID: S-1-5-21-1111111111-2222222222-3333333333
User RID:   1105
User SID:   S-1-5-21-1111111111-2222222222-3333333333-1105
```

Konsep:

| Item | Arti |
|---|---|
| SID | identifier security principal |
| RID | bagian unik SID dalam domain |
| RID Master | FSMO role yang membagikan RID pool |
| GC | partial replica forest-wide |
| UPN suffix | suffix logon user |

```powershell
# Tampilkan global catalog server.
Get-ADDomainController -Filter {IsGlobalCatalog -eq $true}

# Tampilkan UPN suffix forest.
Get-ADForest | Select-Object UPNSuffixes

# Tampilkan SID domain.
Get-ADDomain | Select-Object DomainSID

# Tampilkan SID user.
Get-ADUser alice -Properties objectSid | Select-Object SamAccountName, SID

# Tampilkan RID Master.
Get-ADDomain | Select-Object RIDMaster
```

---

## 2.0 Domain Controller and DNS

### 2.1 Domain Controller Role

Domain controller atau DC adalah server yang menjalankan AD DS dan menyimpan salinan directory database. DC melayani authentication, LDAP query, Kerberos ticket, Group Policy discovery, dan replication.

Komponen penting DC:

| Komponen | Fungsi |
|---|---|
| `NTDS.dit` | database AD |
| `SYSVOL` | policy dan script share |
| `NETLOGON` | share untuk logon script dan domain support |
| KDC | Kerberos Key Distribution Center |
| Netlogon | DC locator dan secure channel |
| DNS Server | sering co-located untuk AD-integrated DNS |
| DFSR | replikasi SYSVOL modern |

Port penting DC:

| Port | Protocol | Fungsi |
|---:|---|---|
| 53 | TCP/UDP | DNS |
| 88 | TCP/UDP | Kerberos |
| 123 | UDP | NTP |
| 135 | TCP | RPC endpoint mapper |
| 389 | TCP/UDP | LDAP |
| 445 | TCP | SMB/SYSVOL |
| 464 | TCP/UDP | Kerberos password change |
| 636 | TCP | LDAPS |
| 3268 | TCP | Global Catalog LDAP |
| 3269 | TCP | Global Catalog LDAPS |
| dynamic RPC | TCP | replication dan management |

Domain controller harus diperlakukan sebagai Tier 0 asset. Compromise DC berarti compromise domain.

```powershell
# Tampilkan domain controller dalam domain.
Get-ADDomainController -Filter * | Select-Object HostName, Site, IsGlobalCatalog, IPv4Address

# Tampilkan service penting domain controller.
Get-Service ADWS, DNS, DFSR, KDC, Netlogon

# Cek share SYSVOL dan NETLOGON.
Get-SmbShare | Where-Object Name -in "SYSVOL","NETLOGON"

# Cek lokasi database dan log AD.
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Services\NTDS\Parameters" | Select-Object "DSA Database file", "Database log files path"
```

### 2.2 AD-Integrated DNS and SRV Records

DNS adalah dependency utama AD. Client dan member server menemukan domain controller melalui DNS SRV records. Jika DNS salah, login, domain join, GPO, LDAP, dan Kerberos bisa gagal.

AD-integrated DNS menyimpan zone di AD dan mereplikasikannya antar DC sesuai replication scope.

Record penting:

| Record | Fungsi |
|---|---|
| `_ldap._tcp.dc._msdcs.domain` | menemukan DC LDAP |
| `_kerberos._tcp.domain` | menemukan Kerberos KDC |
| `_gc._tcp.forest` | menemukan Global Catalog |
| A/AAAA DC | mapping nama DC ke IP |
| CNAME DC GUID | dipakai replication |

DNS client rule:

| Host | DNS client sebaiknya |
|---|---|
| Domain member | DNS internal AD |
| Domain controller | DNS internal, biasanya DC lain dan dirinya sendiri sesuai desain |
| Client | DNS internal lewat DHCP |
| DNS server AD | forwarder ke resolver upstream untuk internet |

Jangan set DNS client domain member ke `8.8.8.8` atau DNS publik sebagai primary. DNS publik tidak tahu SRV record AD internal.

```powershell
# Query SRV record LDAP domain controller.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Query SRV record Kerberos.
Resolve-DnsName _kerberos._tcp.lab.local -Type SRV

# Tampilkan AD-integrated zones.
Get-DnsServerZone | Where-Object IsDsIntegrated

# Register ulang DNS record DC.
ipconfig /registerdns

# Restart Netlogon agar DC mencoba register SRV record ulang.
Restart-Service Netlogon
```

### 2.3 Promoting and Demoting Domain Controllers

Promote DC berarti menginstall AD DS dan menjadikan server sebagai domain controller. Demote DC berarti menghapus role domain controller secara benar dari domain.

Syarat promote DC:

| Syarat | Detail |
|---|---|
| hostname final | rename sebelum promote |
| static IP | IP stabil |
| DNS client benar | menunjuk DNS AD yang sudah ada atau dirinya untuk first DC |
| time benar | authentication dan replication sensitif waktu |
| patch | security update baseline |
| disk | lokasi database/log/SYSVOL cukup |
| admin credential | Enterprise/Domain Admin sesuai scenario |

Promote first DC di forest baru berbeda dari tambah DC ke domain existing. First DC membuat forest/domain baru. Additional DC join ke domain yang sudah ada dan replicate directory.

Demote DC harus dilakukan normal jika DC masih hidup. Jangan langsung delete VM DC kecuali mengikuti metadata cleanup dan recovery procedure.

```powershell
# Install AD DS role dan management tools.
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

# Buat forest baru untuk lab.
Install-ADDSForest -DomainName "lab.local" -DomainNetbiosName "LAB" -InstallDns

# Tambahkan domain controller baru ke domain existing.
Install-ADDSDomainController -DomainName "lab.local" -InstallDns

# Demote domain controller secara normal.
Uninstall-ADDSDomainController -DemoteOperationMasterRole

# Cek status AD DS role.
Get-WindowsFeature AD-Domain-Services
```

### 2.4 SYSVOL, NETLOGON, and DFSR

SYSVOL adalah share yang berisi file Group Policy template, scripts, dan data policy yang harus tersedia di semua DC. NETLOGON adalah share yang dipakai untuk logon script dan fungsi domain tertentu.

Modern Windows Server memakai DFSR untuk replikasi SYSVOL. Environment lama mungkin pernah memakai FRS, tetapi FRS sudah legacy dan harus dimigrasi jika masih ada.

Komponen:

| Komponen | Fungsi |
|---|---|
| SYSVOL share | menyajikan policy files |
| NETLOGON share | logon scripts dan domain support |
| DFSR | replikasi SYSVOL |
| GPT | Group Policy Template di SYSVOL |
| GPC | Group Policy Container di AD |

GPO sehat jika GPC di AD dan GPT di SYSVOL konsisten. Jika AD replication sehat tetapi SYSVOL DFSR rusak, GPO bisa tidak apply.

```powershell
# Cek share SYSVOL dan NETLOGON.
net share

# Cek DFS Replication service.
Get-Service DFSR

# Cek event log DFS Replication terbaru.
Get-WinEvent -LogName "DFS Replication" -MaxEvents 20

# Cek state SYSVOL migration dari FRS ke DFSR.
dfsrmig /getglobalstate

# Cek state lokal migration SYSVOL.
dfsrmig /getmigrationstate
```

### 2.5 Domain Controller Health Checks

Health check DC harus mencakup DNS, replication, SYSVOL, time, service, event log, FSMO, disk, dan secure channel.

Checklist DC:

| Area | Cek |
|---|---|
| DNS | SRV record, zone, forwarder |
| Replication | `repadmin /replsummary` |
| Services | ADWS, DNS, DFSR, KDC, Netlogon |
| SYSVOL | share dan DFSR event |
| Time | `w32tm /query /status` |
| FSMO | holder online |
| Event | Directory Service, DNS Server, DFS Replication, System |
| Disk | free space untuk DB/log/SYSVOL |

```powershell
# Jalankan diagnostic domain controller.
dcdiag

# Jalankan diagnostic DNS khusus DC.
dcdiag /test:dns

# Ringkas status replication forest/domain.
repadmin /replsummary

# Tampilkan replication partner DC.
repadmin /showrepl

# Cek time source DC.
w32tm /query /source

# Cek service penting DC.
Get-Service ADWS, DNS, DFSR, KDC, Netlogon | Select-Object Name, Status, StartType
```

---

## 3.0 Identity Objects

### 3.1 Users

User object merepresentasikan manusia, admin, service legacy, atau identity lain yang perlu login atau mengakses resource. User harus dikelola berdasarkan lifecycle: request, approval, creation, modification, disable, archive, delete.

Attribute penting:

| Attribute | Fungsi |
|---|---|
| `sAMAccountName` | logon legacy |
| `userPrincipalName` | logon modern |
| `displayName` | nama tampilan |
| `mail` | email |
| `department` | departemen |
| `manager` | manager |
| `enabled` | status aktif |
| `memberOf` | group membership langsung |
| `lastLogonTimestamp` | estimasi logon terakhir replicated |
| `pwdLastSet` | waktu password terakhir diganti |

`lastLogon` tidak replicated antar DC. `lastLogonTimestamp` replicated tetapi tidak real-time. Untuk investigasi akurat, query semua DC atau gunakan log SIEM.

Jenis user:

| Jenis | Catatan |
|---|---|
| regular user | user manusia harian |
| admin user | account admin terpisah dari user harian |
| break-glass | emergency account dengan kontrol ketat |
| service user legacy | sebaiknya diganti gMSA jika memungkinkan |
| disabled user | account nonaktif untuk retention |

```powershell
# Buat user baru dalam OU Users.
New-ADUser -Name "Alice Tan" -SamAccountName "alice" -UserPrincipalName "alice@lab.local" -Path "OU=Users,DC=lab,DC=local" -Enabled $true

# Set password user.
Set-ADAccountPassword -Identity alice -Reset -NewPassword (Read-Host -AsSecureString "New password")

# Paksa user mengganti password saat login berikutnya.
Set-ADUser alice -ChangePasswordAtLogon $true

# Tampilkan user dengan attribute penting.
Get-ADUser alice -Properties displayName, mail, department, lastLogonTimestamp, pwdLastSet, memberOf

# Disable user saat offboarding.
Disable-ADAccount alice
```

### 3.2 Groups

Group dipakai untuk authorization dan management. Jangan memberikan permission langsung ke user jika bisa memakai group. Group membuat akses lebih mudah diaudit dan dikelola.

Group type:

| Type | Fungsi |
|---|---|
| Security | bisa dipakai untuk permission |
| Distribution | email distribution, bukan permission |

Group scope:

| Scope | Bisa berisi | Dipakai untuk |
|---|---|---|
| Global | principal dari domain yang sama | role/user grouping |
| Domain Local | principal dari domain mana pun yang trusted | permission ke resource dalam domain |
| Universal | principal dari forest | akses lintas domain/forest |

Model AGDLP:

```text
Account -> Global Group -> Domain Local Group -> Permission
```

Model AGUDLP untuk multi-domain:

```text
Account -> Global Group -> Universal Group -> Domain Local Group -> Permission
```

Contoh:

| Layer | Contoh |
|---|---|
| Account | `alice` |
| Global Group | `GG-Finance-Users` |
| Domain Local Group | `DL-FS-Finance-Modify` |
| Permission | NTFS Modify pada folder Finance |

```powershell
# Buat global security group untuk role user.
New-ADGroup -Name "GG-Finance-Users" -GroupScope Global -GroupCategory Security -Path "OU=Groups,DC=lab,DC=local"

# Buat domain local group untuk permission resource.
New-ADGroup -Name "DL-FS-Finance-Modify" -GroupScope DomainLocal -GroupCategory Security -Path "OU=Groups,DC=lab,DC=local"

# Masukkan user ke global group.
Add-ADGroupMember -Identity "GG-Finance-Users" -Members alice

# Masukkan global group ke domain local group.
Add-ADGroupMember -Identity "DL-FS-Finance-Modify" -Members "GG-Finance-Users"

# Tampilkan membership group.
Get-ADGroupMember "GG-Finance-Users"
```

### 3.3 Computers and Secure Channel

Computer object merepresentasikan mesin yang join domain. Saat join domain, komputer punya machine account password dan secure channel dengan domain. Secure channel dipakai untuk komunikasi trust antara komputer dan domain.

Attribute penting:

| Attribute | Fungsi |
|---|---|
| `dNSHostName` | FQDN komputer |
| `servicePrincipalName` | SPN host/service |
| `operatingSystem` | OS reported |
| `lastLogonTimestamp` | estimasi aktivitas |
| `userAccountControl` | flag account komputer |
| `msDS-SupportedEncryptionTypes` | encryption Kerberos yang didukung |

Secure channel bisa rusak jika snapshot rollback, clone tanpa sysprep, machine password mismatch, atau object komputer reset/delete.

```powershell
# Tampilkan computer object.
Get-ADComputer LAB-FS-01 -Properties dNSHostName, operatingSystem, lastLogonTimestamp, servicePrincipalName

# Test secure channel dari member server/client.
Test-ComputerSecureChannel

# Repair secure channel dengan credential domain.
Test-ComputerSecureChannel -Repair -Credential LAB\AdminUser

# Join komputer ke domain.
Add-Computer -DomainName lab.local -Restart
```

Catatan: computer account biasa berbeda dari managed service account. Untuk secure channel komputer biasa, gunakan `Test-ComputerSecureChannel -Repair`, `Reset-ComputerMachinePassword`, `netdom resetpwd`, atau rejoin domain sesuai scenario.

```powershell
# Reset password secure channel komputer lokal memakai netdom.
netdom resetpwd /server:LAB-DC-01 /userd:LAB\AdminUser /passwordd:*

# Reset machine account password dari komputer lokal.
Reset-ComputerMachinePassword -Server LAB-DC-01 -Credential LAB\AdminUser

# Keluar dari domain ke workgroup jika perlu rejoin.
Remove-Computer -UnjoinDomainCredential LAB\AdminUser -WorkgroupName WORKGROUP -Restart
```

### 3.4 Service Accounts, SPN, and gMSA

Service account dipakai aplikasi/service untuk authentication. Risiko utama adalah password statis, privilege berlebihan, SPN salah, dan delegation yang terlalu luas.

Jenis:

| Jenis | Catatan |
|---|---|
| domain user service account | umum tapi password harus dikelola |
| Managed Service Account | managed password untuk satu host |
| gMSA | group managed service account untuk multi-host/service |
| computer account | dipakai service bawaan host |
| virtual account | local managed identity untuk service tertentu |

SPN atau Service Principal Name memetakan service instance ke account yang menjalankan service. Kerberos butuh SPN untuk menerbitkan service ticket.

Format SPN:

```text
serviceclass/host:port
HTTP/web01.lab.local
MSSQLSvc/sql01.lab.local:1433
```

SPN duplicate bisa menyebabkan Kerberos gagal dan fallback ke NTLM.

```powershell
# Tampilkan SPN pada user service account.
Get-ADUser svc-web -Properties servicePrincipalName | Select-Object -ExpandProperty servicePrincipalName

# Tambahkan SPN HTTP ke service account.
setspn -S HTTP/web01.lab.local LAB\svc-web

# Cari duplicate SPN.
setspn -X

# Buat KDS root key untuk gMSA lab.
Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))

# Buat gMSA untuk web farm.
New-ADServiceAccount -Name gmsa-web -DNSHostName gmsa-web.lab.local -PrincipalsAllowedToRetrieveManagedPassword "GG-WebServers"
```

### 3.5 Contacts, Disabled Objects, and Lifecycle

Tidak semua object identity adalah user aktif. Contact object dipakai untuk representasi external email/contact tanpa login. Disabled object dipakai saat retention sebelum deletion.

Lifecycle penting:

| Tahap | Praktik |
|---|---|
| Joiner | buat user, group, mailbox/app access |
| Mover | update department, manager, group, OU |
| Leaver | disable, remove session/token, revoke access |
| Retention | pindahkan ke OU disabled, simpan sesuai policy |
| Delete | hapus setelah retention dan backup/restore path jelas |

Offboarding yang baik:

- disable account
- reset password
- remove dari privileged groups
- revoke session/cloud token jika hybrid
- pindahkan ke OU disabled
- catat owner dan tanggal
- review mailbox/data handover

```powershell
# Cari user disabled.
Search-ADAccount -AccountDisabled -UsersOnly

# Cari user yang tidak login dalam 90 hari berdasarkan lastLogonTimestamp.
Search-ADAccount -AccountInactive -UsersOnly -TimeSpan 90.00:00:00

# Pindahkan user disabled ke OU Disabled Users.
Move-ADObject -Identity (Get-ADUser alice).DistinguishedName -TargetPath "OU=Disabled Users,DC=lab,DC=local"

# Set deskripsi offboarding.
Set-ADUser alice -Description "Disabled on 2026-05-24 - offboarding ticket INC000123"
```

---

## 4.0 Authentication and Authorization

### 4.1 Kerberos Flow

Kerberos adalah authentication protocol utama AD. Kerberos memakai ticket, bukan mengirim password ke service setiap kali akses.

Komponen Kerberos:

| Komponen | Fungsi |
|---|---|
| Client | user/computer yang meminta akses |
| KDC | Key Distribution Center di DC |
| AS | Authentication Service, menerbitkan TGT |
| TGS | Ticket Granting Service, menerbitkan service ticket |
| TGT | Ticket Granting Ticket |
| TGS ticket | ticket untuk service tertentu |
| SPN | nama service untuk ticket |

Alur sederhana:

1. User login dan client meminta TGT ke KDC.
2. KDC memvalidasi identity dan memberi TGT.
3. Client ingin akses service, misalnya `\\fs01\share`.
4. Client meminta service ticket untuk SPN `cifs/fs01.lab.local`.
5. KDC memberi service ticket.
6. Client menunjukkan service ticket ke file server.
7. File server memvalidasi ticket dan membuat access token.
8. Akses resource ditentukan oleh ACL dan group membership.

```text
Client -> KDC AS: minta TGT
KDC AS -> Client: TGT
Client -> KDC TGS: minta service ticket untuk SPN
KDC TGS -> Client: service ticket
Client -> Service: present service ticket
Service -> Client: access diterima/ditolak berdasarkan authorization
```

Kerberos sensitif terhadap:

- DNS name dan SPN
- time skew
- encryption type
- account password/service key
- delegation setting
- duplicate SPN
- domain trust

```powershell
# Tampilkan ticket Kerberos saat ini.
klist

# Hapus ticket cache Kerberos user saat ini.
klist purge

# Query SPN CIFS file server.
setspn -Q cifs/fs01.lab.local

# Cek time source.
w32tm /query /source

# Test akses SMB yang memicu Kerberos/NTLM.
dir \\fs01.lab.local\share
```

### 4.2 NTLM and Legacy Authentication

NTLM adalah authentication protocol lama. NTLM masih ada untuk compatibility, tetapi lebih lemah dari Kerberos dalam banyak scenario modern dan sering menjadi risiko relay/pass-the-hash jika environment tidak dikontrol.

NTLM biasanya muncul ketika:

| Penyebab | Contoh |
|---|---|
| akses pakai IP | `\\10.10.10.20\share` |
| SPN tidak ada/salah | service custom |
| workgroup/non-domain | tidak ada Kerberos domain |
| trust/path bermasalah | Kerberos tidak bisa menyelesaikan |
| legacy app | hanya mendukung NTLM |

Strategi defense:

- inventaris penggunaan NTLM sebelum disable
- aktifkan audit NTLM
- kurangi akses memakai IP
- perbaiki SPN
- gunakan SMB signing/LDAP signing/channel binding sesuai desain
- batasi NTLM via policy bertahap

```powershell
# Tampilkan policy NTLM-related dari registry policy jika ada.
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0" -ErrorAction SilentlyContinue

# Cari event NTLM Operational jika log tersedia.
Get-WinEvent -LogName "Microsoft-Windows-NTLM/Operational" -MaxEvents 20

# Cek apakah SMB signing required di server.
Get-SmbServerConfiguration | Select-Object RequireSecuritySignature, EnableSecuritySignature
```

### 4.3 LDAP, LDAPS, and Directory Query

LDAP dipakai untuk query dan modify directory. Banyak aplikasi memakai LDAP bind untuk authentication atau lookup user/group. LDAPS adalah LDAP over TLS, biasanya port 636 untuk domain controller dan 3269 untuk global catalog.

Port:

| Port | Fungsi |
|---:|---|
| 389 | LDAP |
| 636 | LDAPS |
| 3268 | Global Catalog LDAP |
| 3269 | Global Catalog LDAPS |

LDAP bind:

| Jenis | Catatan |
|---|---|
| simple bind | credential dikirim dalam mekanisme sederhana, harus pakai TLS |
| SASL/Kerberos | lebih aman di domain |
| anonymous bind | sebaiknya dibatasi |

LDAPS membutuhkan certificate valid di DC. Certificate harus punya Server Authentication EKU, private key, dan subject/SAN yang cocok.

```powershell
# Test port LDAP ke DC.
Test-NetConnection LAB-DC-01 -Port 389

# Test port LDAPS ke DC.
Test-NetConnection LAB-DC-01 -Port 636

# Query user dengan LDAP filter.
Get-ADUser -LDAPFilter "(&(objectClass=user)(sAMAccountName=alice))"

# Query group dengan LDAP filter.
Get-ADGroup -LDAPFilter "(cn=GG-Finance-Users)"

# Tampilkan certificate di LocalMachine My store pada DC.
Get-ChildItem Cert:\LocalMachine\My
```

### 4.4 Access Token, SID, Group Membership, and ACL

Authorization di Windows memakai access token dan ACL. Saat user login, Windows membuat token berisi SID user, group SID, privilege, integrity level, dan informasi logon lain. Saat user akses resource, Windows membandingkan token dengan security descriptor resource.

Komponen:

| Komponen | Fungsi |
|---|---|
| SID user | identity user |
| SID group | membership group |
| privilege | hak seperti backup/restore/debug |
| DACL | daftar allow/deny ACE |
| SACL | audit access |
| ACE | entry permission |
| owner | pemilik object |

Group membership biasanya masuk token saat logon. Jika user baru ditambahkan ke group, user sering perlu logoff/logon ulang agar token baru memuat group tersebut.

Token bloat terjadi saat user terlalu banyak group, menyebabkan token besar dan bisa memicu masalah authentication/access.

```powershell
# Tampilkan identity user saat ini.
whoami

# Tampilkan group dalam token user saat ini.
whoami /groups

# Tampilkan privilege dalam token user saat ini.
whoami /priv

# Tampilkan group membership user dari AD.
Get-ADPrincipalGroupMembership alice | Select-Object Name, GroupScope, GroupCategory

# Tampilkan ACL folder share.
icacls "\\fs01\finance"
```

### 4.5 Time, Password Policy, and Account Lockout

Kerberos membutuhkan waktu yang sinkron. Selisih waktu terlalu jauh bisa membuat authentication gagal. Di domain, PDC Emulator punya peran penting sebagai time source utama domain.

Password policy default domain berlaku di level domain. Fine-Grained Password Policy atau FGPP bisa memberi policy berbeda untuk user/group tertentu.

Account lockout melindungi dari brute force tetapi juga bisa menjadi denial-of-service jika threshold terlalu agresif atau ada service lama memakai password salah.

Konsep:

| Konsep | Fungsi |
|---|---|
| PDC Emulator | time source utama domain dan proses password tertentu |
| default domain policy | password/lockout policy domain |
| FGPP | password policy granular |
| badPwdCount | counter gagal per DC |
| lockoutTime | waktu account terkunci |
| pwdLastSet | password terakhir diganti |

```powershell
# Tampilkan password policy domain.
Get-ADDefaultDomainPasswordPolicy

# Tampilkan fine-grained password policies.
Get-ADFineGrainedPasswordPolicy -Filter *

# Cek status account user.
Get-ADUser alice -Properties LockedOut, lockoutTime, badPwdCount, pwdLastSet

# Unlock account user.
Unlock-ADAccount alice

# Cek PDC Emulator domain.
Get-ADDomain | Select-Object PDCEmulator

# Cek time status.
w32tm /query /status
```

---

## 5.0 Group Policy

### 5.1 GPO Components and Processing

Group Policy mengatur konfigurasi user dan computer secara terpusat. GPO punya dua bagian:

| Bagian | Lokasi | Fungsi |
|---|---|---|
| GPC | AD | metadata GPO |
| GPT | SYSVOL | file policy, template, script |

GPO bisa berisi:

| Area | Contoh |
|---|---|
| Security Settings | password policy, audit, user rights |
| Administrative Templates | registry-based policy |
| Preferences | drive map, registry, files, scheduled tasks |
| Scripts | startup/shutdown/logon/logoff |
| Software Installation | legacy MSI deployment |
| Folder Redirection | redirect folder user |
| Windows Defender/Firewall | endpoint security settings |

Order processing umum:

```text
Local GPO -> Site GPO -> Domain GPO -> OU GPO dari parent ke child
```

Sering disebut LSDOU: Local, Site, Domain, OU.

```powershell
# Import module GroupPolicy.
Import-Module GroupPolicy

# Tampilkan semua GPO.
Get-GPO -All

# Buat GPO baru.
New-GPO -Name "Server Baseline"

# Link GPO ke OU Servers.
New-GPLink -Name "Server Baseline" -Target "OU=Servers,DC=lab,DC=local"

# Buat report GPO HTML.
Get-GPOReport -Name "Server Baseline" -ReportType Html -Path C:\Temp\Server-Baseline.html
```

### 5.2 Link Order, Inheritance, Enforced, and Block Inheritance

GPO bisa di-link ke site, domain, atau OU. Link order menentukan prioritas pada level yang sama. Inheritance membuat GPO parent turun ke child OU.

Konsep:

| Konsep | Arti |
|---|---|
| link enabled | GPO link aktif |
| link order | urutan prioritas pada container yang sama |
| inheritance | GPO parent berlaku ke child |
| block inheritance | child OU menolak inheritance biasa |
| enforced | GPO tidak bisa diblokir dan punya prioritas |
| disabled user/computer config | mematikan separuh GPO agar processing lebih efisien |

Enforced harus dipakai hemat. Terlalu banyak enforced GPO membuat troubleshooting sulit dan desain policy berantakan.

```powershell
# Tampilkan inheritance GPO pada OU tertentu.
Get-GPInheritance -Target "OU=Servers,DC=lab,DC=local"

# Set block inheritance pada OU.
Set-GPInheritance -Target "OU=Servers,DC=lab,DC=local" -IsBlocked Yes

# Set GPO link sebagai enforced.
Set-GPLink -Name "Server Baseline" -Target "OU=Servers,DC=lab,DC=local" -Enforced Yes

# Disable bagian user configuration pada GPO server.
Set-GPO -Name "Server Baseline" -GpoStatus UserSettingsDisabled
```

### 5.3 Security Filtering, WMI Filtering, and Item-Level Targeting

Security filtering menentukan principal mana yang boleh apply GPO. Agar GPO apply, principal biasanya perlu `Read` dan `Apply Group Policy`.

WMI filter menentukan GPO apply berdasarkan query WMI, misalnya OS version. WMI filter terlalu kompleks bisa memperlambat processing.

Item-Level Targeting ada di Group Policy Preferences dan memungkinkan setting tertentu apply berdasarkan kondisi seperti group membership, OS, IP range, registry, atau file exists.

Perbedaan:

| Mekanisme | Level | Contoh |
|---|---|---|
| Security filtering | GPO | hanya group server tertentu |
| WMI filtering | GPO | hanya Windows Server 2022 |
| Item-level targeting | preference item | drive map hanya untuk group tertentu |

```powershell
# Tampilkan permission GPO.
Get-GPPermission -Name "Server Baseline" -All

# Beri permission apply GPO ke group tertentu.
Set-GPPermission -Name "Server Baseline" -TargetName "GG-Servers-Baseline" -TargetType Group -PermissionLevel GpoApply

# Hapus Apply Group Policy dari Authenticated Users tetapi tetap hati-hati dengan Read permission.
Set-GPPermission -Name "Server Baseline" -TargetName "Authenticated Users" -TargetType Group -PermissionLevel GpoRead

# Tampilkan WMI filters.
Get-GPWmiFilter -All
```

### 5.4 Loopback Processing

Loopback processing membuat user policy dipengaruhi oleh lokasi computer object. Ini sering dipakai untuk RDS, kiosk, lab, jump server, atau shared workstation.

Mode:

| Mode | Arti |
|---|---|
| Merge | user policy normal digabung dengan user policy dari computer OU |
| Replace | user policy normal diganti oleh user policy dari computer OU |

Contoh: user `alice` login ke RDS server. Walaupun user berada di OU `Users`, loopback di OU `RDS Servers` bisa menerapkan user settings khusus RDS.

Hati-hati:

- loopback bisa membuat policy terasa "misterius"
- dokumentasikan OU yang memakai loopback
- gunakan RSOP/gpresult untuk membuktikan sumber policy

```powershell
# Buat registry policy loopback replace pada GPO.
Set-GPRegistryValue -Name "RDS User Lockdown" -Key "HKLM\Software\Policies\Microsoft\Windows\System" -ValueName "UserPolicyMode" -Type DWord -Value 2

# Link GPO loopback ke OU RDS Servers.
New-GPLink -Name "RDS User Lockdown" -Target "OU=RDS Servers,OU=Servers,DC=lab,DC=local"

# Cek hasil policy pada client/server.
gpresult /r
```

### 5.5 GPO Troubleshooting

GPO tidak apply biasanya disebabkan scope salah, security filtering, WMI filter, replication, SYSVOL, DNS, slow link, permission, atau conflict policy.

Checklist:

| Pertanyaan | Cek |
|---|---|
| object ada di OU yang benar | ADUC/PowerShell |
| GPO link enabled | GPMC |
| GPO apply permission benar | security filtering |
| user/computer config disabled | GPO status |
| WMI filter true | query WMI |
| SYSVOL reachable | `\\domain\SYSVOL` |
| DC yang dipakai punya policy terbaru | replication |
| client sudah update policy | `gpupdate` |

```powershell
# Force update Group Policy.
gpupdate /force

# Tampilkan ringkasan GPO applied.
gpresult /r

# Export hasil policy ke HTML.
gpresult /h C:\Temp\gpresult.html

# Tampilkan event Group Policy terbaru.
Get-WinEvent -LogName "Microsoft-Windows-GroupPolicy/Operational" -MaxEvents 30

# Cek akses SYSVOL domain.
dir \\lab.local\SYSVOL
```

---

## 6.0 Replication, Sites, and FSMO

### 6.1 AD Replication Concepts

AD adalah multi-master directory. Banyak DC bisa menerima perubahan, lalu perubahan direplikasi ke DC lain. Namun beberapa role khusus tetap single-master melalui FSMO.

Partition/naming context:

| Partition | Isi |
|---|---|
| Schema | definisi object/attribute forest-wide |
| Configuration | konfigurasi forest, sites, services |
| Domain | object domain seperti user/group/computer |
| Application | data aplikasi seperti DNS zone tertentu |

Replication metadata melacak perubahan attribute. AD replication bukan file copy biasa. Ia berbasis update sequence number, invocation ID, high-watermark, dan up-to-dateness vector.

Konsep:

| Konsep | Arti |
|---|---|
| intra-site replication | replication dalam site, cepat dan sering |
| inter-site replication | antar site, mengikuti schedule/cost |
| change notification | DC memberi tahu perubahan |
| pull replication | partner menarik perubahan |
| replication topology | graph connection antar DC |
| KCC | Knowledge Consistency Checker pembuat topology |

```powershell
# Ringkas status replication.
repadmin /replsummary

# Tampilkan replication inbound detail.
repadmin /showrepl

# Tampilkan replication queue.
repadmin /queue

# Tampilkan replication metadata object tertentu.
repadmin /showobjmeta LAB-DC-01 "CN=Alice Tan,OU=Users,DC=lab,DC=local"
```

### 6.2 Sites, Subnets, Site Links, and KCC

Site merepresentasikan lokasi network dengan koneksi cepat dan andal. Subnet AD dipetakan ke site agar client memilih DC terdekat. Jika subnet tidak didaftarkan, client bisa memakai DC jauh dan login/GPO menjadi lambat.

Komponen:

| Komponen | Fungsi |
|---|---|
| Site | lokasi network/logical |
| Subnet | prefix IP yang dipetakan ke site |
| Site link | jalur replication antar site |
| Cost | prioritas link |
| Schedule | kapan replication antar site boleh terjadi |
| Bridgehead server | DC yang mewakili replication antar site |
| KCC | membangun connection object |

Contoh:

| Subnet | Site |
|---|---|
| `10.10.10.0/24` | Jakarta |
| `10.20.10.0/24` | Singapore |
| `10.30.10.0/24` | Tokyo |

```powershell
# Tampilkan site AD.
Get-ADReplicationSite -Filter *

# Buat site baru.
New-ADReplicationSite -Name "Jakarta"

# Buat subnet dan map ke site.
New-ADReplicationSubnet -Name "10.10.10.0/24" -Site "Jakarta"

# Tampilkan site link.
Get-ADReplicationSiteLink -Filter *

# Paksa KCC menghitung ulang topology pada DC.
repadmin /kcc LAB-DC-01
```

### 6.3 Repadmin and Replication Troubleshooting

Replication troubleshooting harus dimulai dari ringkasan lalu masuk ke DC/partition yang gagal.

Gejala replication rusak:

| Gejala | Kemungkinan |
|---|---|
| user baru belum muncul di site lain | inter-site schedule/cost atau replication error |
| GPO beda antar DC | AD replication atau SYSVOL DFSR |
| password reset tidak berlaku | PDC/replication/auth ke DC lama |
| domain join gagal di site tertentu | DNS/SRV/site mapping |
| event Directory Service error | replication topology, DNS, RPC, lingering |

Command penting:

```powershell
# Lihat ringkasan replication error.
repadmin /replsummary

# Lihat detail replication inbound semua DC.
repadmin /showrepl *

# Paksa sinkronisasi semua partition.
repadmin /syncall /AdeP

# Tampilkan partner replication.
repadmin /showconn

# Cek replication failures via PowerShell.
Get-ADReplicationFailure -Target (Get-ADDomain).DNSRoot -Scope Domain
```

### 6.4 FSMO Roles

FSMO atau Flexible Single Master Operations adalah role AD yang tidak multi-master. Ada 5 role:

| FSMO | Scope | Fungsi |
|---|---|---|
| Schema Master | forest | perubahan schema |
| Domain Naming Master | forest | tambah/hapus domain dalam forest |
| RID Master | domain | membagikan RID pool |
| PDC Emulator | domain | time source, password change priority, lockout, legacy PDC |
| Infrastructure Master | domain | referensi object antar domain |

PDC Emulator sering paling terasa operasional karena terkait time, password changes, account lockout, dan beberapa tool management.

Transfer vs seize:

| Operasi | Kapan |
|---|---|
| transfer | DC lama masih online dan sehat |
| seize | DC holder mati permanen dan tidak akan kembali |

Seize FSMO harus hati-hati. Jangan menghidupkan kembali DC lama yang role-nya sudah di-seize tanpa cleanup yang benar.

```powershell
# Tampilkan semua FSMO holder.
netdom query fsmo

# Tampilkan FSMO domain.
Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster

# Tampilkan FSMO forest.
Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster

# Transfer FSMO role ke DC target.
Move-ADDirectoryServerOperationMasterRole -Identity LAB-DC-02 -OperationMasterRole PDCEmulator, RIDMaster, InfrastructureMaster
```

### 6.5 RID Pool, USN, Tombstone, and Lingering Objects

RID pool adalah kumpulan RID yang diberikan RID Master ke DC agar DC bisa membuat security principal baru. Jika RID pool bermasalah, pembuatan user/group/computer bisa gagal.

USN atau Update Sequence Number dipakai DC untuk melacak perubahan. Snapshot rollback DC lama bisa menyebabkan USN rollback dan replication corruption jika virtualization safeguards tidak mendukung atau prosedur salah.

Tombstone adalah object yang sudah dihapus tetapi masih disimpan sementara agar deletion replicate ke DC lain. Lingering object bisa muncul jika DC offline melewati tombstone lifetime lalu kembali.

Konsep:

| Konsep | Arti |
|---|---|
| RID pool | alokasi RID untuk DC |
| USN | nomor urut perubahan lokal DC |
| invocation ID | identitas instance database DC |
| tombstone lifetime | masa object deleted disimpan |
| lingering object | object usang yang tidak seharusnya ada |

```powershell
# Cek RID Master.
Get-ADDomain | Select-Object RIDMaster

# Tampilkan metadata replication object.
repadmin /showobjmeta LAB-DC-01 "CN=Alice Tan,OU=Users,DC=lab,DC=local"

# Tampilkan up-to-dateness vector.
repadmin /showutdvec LAB-DC-01 "DC=lab,DC=local"

# Cek event Directory Service terbaru.
Get-WinEvent -LogName "Directory Service" -MaxEvents 30
```

---

## 7.0 Administration and Delegation

### 7.1 Administrative Tools

AD bisa dikelola dengan GUI dan command line. GUI bagus untuk eksplorasi dan operasi kecil, PowerShell lebih baik untuk audit, bulk operation, dan perubahan repeatable.

Tools:

| Tool | Fungsi |
|---|---|
| ADUC | Active Directory Users and Computers |
| ADAC | Active Directory Administrative Center |
| GPMC | Group Policy Management Console |
| DNS Manager | manage DNS AD-integrated |
| AD Sites and Services | manage site, subnet, replication topology |
| ADSI Edit | low-level editor, berbahaya jika tidak hati-hati |
| LDP.exe | LDAP troubleshooting |
| PowerShell AD module | automation |
| Repadmin | replication troubleshooting |
| DCDiag | DC diagnostics |

ADSI Edit bisa merusak AD jika salah edit. Pakai hanya saat paham attribute dan punya backup/recovery plan.

```powershell
# Install RSAT AD tools pada server/client yang mendukung.
Install-WindowsFeature RSAT-AD-Tools

# Tampilkan command module Active Directory.
Get-Command -Module ActiveDirectory

# Buka ADUC.
dsa.msc

# Buka GPMC.
gpmc.msc

# Buka AD Sites and Services.
dssite.msc
```

### 7.2 OU Design

OU design harus mendukung policy, delegation, dan operasi. Jangan hanya meniru struktur organisasi HR karena struktur HR sering berubah dan tidak selalu cocok untuk GPO/security.

Prinsip:

| Prinsip | Alasan |
|---|---|
| pisahkan users dan computers | policy user/computer berbeda |
| pisahkan servers dan workstations | baseline security berbeda |
| pisahkan domain controllers | Tier 0 dan default OU khusus |
| pisahkan admin accounts | privilege dan monitoring berbeda |
| pisahkan disabled objects | lifecycle jelas |
| buat OU berdasarkan policy/delegation | mengurangi GPO/filter rumit |

Contoh desain ringkas:

```text
OU=Tier0
  OU=Admin Accounts
  OU=Admin Workstations
OU=Servers
  OU=File Servers
  OU=Web Servers
  OU=Database Servers
OU=Workstations
  OU=Standard
  OU=Kiosk
OU=Users
  OU=Employees
  OU=Contractors
OU=Groups
OU=Disabled Objects
```

```powershell
# Tampilkan OU beserta parent path.
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName

# Buat OU Disabled Objects dengan proteksi accidental deletion.
New-ADOrganizationalUnit -Name "Disabled Objects" -Path "DC=lab,DC=local" -ProtectedFromAccidentalDeletion $true

# Cek object yang dilindungi dari accidental deletion.
Get-ADOrganizationalUnit -Filter * -Properties ProtectedFromAccidentalDeletion | Where-Object ProtectedFromAccidentalDeletion
```

### 7.3 Delegation Model

Delegation berarti memberi admin terbatas hak mengelola object tertentu tanpa memberi privilege domain-wide. Delegation yang baik mengurangi kebutuhan Domain Admin.

Contoh delegation:

| Tim | Hak |
|---|---|
| Helpdesk | reset password user non-admin |
| Desktop team | join computer ke domain dan move workstation OU |
| Server team | manage server computer objects |
| HR app team | update attribute tertentu |
| DNS team | manage zone tertentu |

Delegation harus:

- berbasis group, bukan user individual
- terbatas ke OU/scope tertentu
- terdokumentasi
- diaudit
- direview berkala
- tidak mencakup admin Tier 0 kecuali benar-benar perlu

```powershell
# Tampilkan ACL pada OU.
Get-Acl "AD:\OU=Users,DC=lab,DC=local" | Format-List

# Buka delegation wizard via ADUC untuk operasi delegation umum.
dsa.msc

# Cari group yang diberi delegation berdasarkan nama.
Get-ADGroup -Filter 'Name -like "*Helpdesk*"' -Properties Description

# Tampilkan member group helpdesk.
Get-ADGroupMember "GG-Helpdesk-PasswordReset"
```

### 7.4 Bulk Administration with PowerShell

Bulk operation harus aman, repeatable, dan bisa diuji. Jangan menjalankan perubahan massal tanpa `-WhatIf`, sample data, dan log output.

Prinsip bulk change:

1. export data sebelum perubahan
2. test pada beberapa object
3. pakai `-WhatIf` jika tersedia
4. log perubahan
5. gunakan filter spesifik
6. siapkan rollback

Contoh CSV user:

```text
SamAccountName,Name,UPN,Department
alice,Alice Tan,alice@lab.local,Finance
bob,Bob Lim,bob@lab.local,IT
```

```powershell
# Preview import CSV user.
Import-Csv C:\Temp\users.csv | Select-Object -First 5

# Buat user dari CSV dengan WhatIf.
Import-Csv C:\Temp\users.csv | ForEach-Object {
    New-ADUser -Name $_.Name -SamAccountName $_.SamAccountName -UserPrincipalName $_.UPN -Department $_.Department -Path "OU=Users,DC=lab,DC=local" -WhatIf
}

# Export user aktif ke CSV.
Get-ADUser -Filter {Enabled -eq $true} -Properties Department, Mail | Select-Object SamAccountName, Name, Department, Mail | Export-Csv C:\Temp\active-users.csv -NoTypeInformation

# Cari user tanpa manager.
Get-ADUser -Filter * -Properties Manager | Where-Object {-not $_.Manager} | Select-Object SamAccountName, Name
```

### 7.5 Change Control and Documentation

AD change bisa berdampak luas. Perubahan kecil seperti mengubah GPO, DNS, group privileged, atau delegation bisa memutus login atau membuka akses.

Perubahan yang harus punya change record:

| Perubahan | Risiko |
|---|---|
| GPO baseline | bisa memengaruhi banyak endpoint/server |
| DNS zone/forwarder | bisa memutus name resolution |
| FSMO transfer | operasi domain/forest |
| DC demotion | kapasitas auth/replication |
| group privileged | privilege escalation |
| delegation | akses admin tersembunyi |
| trust | boundary antar domain/forest |
| schema extension | forest-wide |

Dokumentasi minimum:

- alasan perubahan
- object yang diubah
- scope dampak
- waktu perubahan
- approver
- command/screenshot
- validasi
- rollback

```powershell
# Export daftar privileged group members untuk baseline.
"Domain Admins","Enterprise Admins","Schema Admins","Administrators" | ForEach-Object {
    Get-ADGroupMember $_ | Select-Object @{Name="Group";Expression={$_}}, Name, SamAccountName, objectClass
} | Export-Csv C:\Temp\privileged-groups.csv -NoTypeInformation

# Export semua GPO sebagai report XML.
Get-GPO -All | ForEach-Object {
    Get-GPOReport -Guid $_.Id -ReportType Xml -Path "C:\Temp\GPO-$($_.DisplayName).xml"
}

# Backup semua GPO.
Backup-GPO -All -Path C:\Temp\GPO-Backup
```

---

## 8.0 Security Engineering

### 8.1 Tiering Model and Privileged Access

AD security harus dimulai dari privilege model. Tiering memisahkan admin berdasarkan sensitivitas asset.

Model umum:

| Tier | Asset | Contoh admin |
|---|---|---|
| Tier 0 | identity/control plane | DC, AD, PKI, ADFS, Entra Connect, privileged groups |
| Tier 1 | server/application | member server, database, file server |
| Tier 2 | workstation/user endpoint | client device dan user support |

Aturan:

- Tier 0 admin tidak login ke Tier 1/2.
- Tier 1 admin tidak punya hak ke Tier 0.
- Tier 2 admin tidak mengelola server/DC.
- Admin account dipisah dari user harian.
- Privileged Access Workstation atau jump host dipakai untuk admin sensitif.

Mengapa penting: jika Domain Admin login ke workstation biasa yang compromise, credential/token bisa dicuri dan domain bisa jatuh.

```powershell
# Tampilkan member Domain Admins.
Get-ADGroupMember "Domain Admins"

# Tampilkan member Enterprise Admins.
Get-ADGroupMember "Enterprise Admins"

# Cari user admin berdasarkan naming convention.
Get-ADUser -Filter 'SamAccountName -like "adm-*"' -Properties Enabled, LastLogonTimestamp

# Tampilkan logon admin dari Security log berdasarkan event privileged logon.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4672} -MaxEvents 50
```

### 8.2 Domain Controller Hardening

Domain controller adalah asset paling sensitif. Hardening DC harus ketat dan diuji.

Baseline DC:

| Area | Praktik |
|---|---|
| Role isolation | DC tidak menjalankan workload umum |
| Admin access | hanya Tier 0 admin |
| RDP | dibatasi dari PAW/jump host |
| Internet | tidak browsing/download langsung |
| Patch | prioritas tinggi |
| AV/EDR | aktif dengan exclusion yang benar |
| Firewall | inbound sesuai role DC |
| Audit | advanced audit aktif |
| Backup | system state teruji |
| Physical/VM access | hypervisor admin dianggap Tier 0 untuk DC VM |

DC VM perlu proteksi dari snapshot rollback sembarangan, backup tidak aman, dan admin hypervisor yang terlalu luas. Siapa pun yang bisa mengakses disk DC bisa mencoba mengambil database AD offline.

```powershell
# Tampilkan local Administrators pada DC.
Get-LocalGroupMember Administrators

# Cek inbound firewall rule aktif pada DC.
Get-NetFirewallRule -Direction Inbound -Enabled True | Select-Object DisplayName, Profile, Action

# Cek status Defender.
Get-MpComputerStatus

# Tampilkan audit policy.
auditpol /get /category:*

# Cek event log cleared.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=1102} -MaxEvents 10
```

### 8.3 Admin Groups and Dangerous Rights

Privileged group harus kecil, diaudit, dan punya owner jelas.

Group sensitif:

| Group | Risiko |
|---|---|
| Enterprise Admins | kontrol forest-wide |
| Domain Admins | kontrol domain |
| Schema Admins | ubah schema forest |
| Administrators | admin built-in domain/DC |
| Account Operators | manage banyak account |
| Server Operators | hak tinggi di DC |
| Backup Operators | bisa membaca data sensitif melalui backup |
| Print Operators | legacy risk pada DC |
| Group Policy Creator Owners | bisa membuat GPO |
| DNSAdmins | bisa berisiko tinggi jika DNS di DC |

Dangerous rights/delegation:

| Right | Risiko |
|---|---|
| GenericAll | kontrol penuh object |
| GenericWrite | bisa ubah attribute berbahaya |
| WriteDACL | bisa mengubah permission |
| WriteOwner | bisa mengambil ownership |
| AllExtendedRights | termasuk reset password di banyak object |
| Replicating Directory Changes | terkait DCSync jika kombinasi tertentu |
| Add member | bisa menambah diri ke group sensitif |

```powershell
# Review member group privileged.
"Enterprise Admins","Domain Admins","Schema Admins","Administrators","Account Operators","Server Operators","Backup Operators","DnsAdmins" | ForEach-Object {
    Get-ADGroupMember $_ -ErrorAction SilentlyContinue | Select-Object @{Name="Group";Expression={$_}}, Name, SamAccountName, objectClass
}

# Tampilkan ACL domain root.
Get-Acl "AD:\DC=lab,DC=local" | Select-Object -ExpandProperty Access

# Cari group yang namanya mengandung admin.
Get-ADGroup -Filter 'Name -like "*Admin*"' | Select-Object Name, DistinguishedName
```

### 8.4 Kerberos Security, Delegation, and SPN Risk

Delegation memungkinkan service bertindak atas nama user ke service lain. Ini kuat tetapi berisiko jika salah desain.

Jenis delegation:

| Jenis | Risiko |
|---|---|
| unconstrained delegation | sangat berisiko, service bisa menerima delegated TGT |
| constrained delegation | dibatasi ke service tertentu |
| resource-based constrained delegation | kontrol di resource/service target |

SPN risk:

- SPN pada user account dengan password lemah meningkatkan risiko offline cracking terhadap service ticket
- duplicate SPN menyebabkan Kerberos gagal
- delegation pada account sensitif bisa membuka jalur lateral movement
- service account privileged memperbesar dampak compromise

Defensive checks:

```powershell
# Cari account dengan unconstrained delegation.
Get-ADObject -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=524288)" -Properties userAccountControl, servicePrincipalName

# Cari user service account dengan SPN.
Get-ADUser -LDAPFilter "(servicePrincipalName=*)" -Properties servicePrincipalName | Select-Object SamAccountName, servicePrincipalName

# Cari protected users group members.
Get-ADGroupMember "Protected Users"
```

Catatan syntax PowerShell: attribute dengan tanda hubung kadang perlu diakses sebagai property string.

```powershell
# Cari computer dengan constrained delegation memakai property string yang aman.
Get-ADComputer -Filter * -Properties "msDS-AllowedToDelegateTo" | Where-Object {$_."msDS-AllowedToDelegateTo"} | Select-Object Name, "msDS-AllowedToDelegateTo"
```

### 8.5 Audit Policy, Event IDs, and Detection

AD security bergantung pada logging yang benar. DC harus mengirim log ke SIEM/log collector agar attacker tidak bisa menghapus bukti lokal begitu saja.

Audit category penting:

| Category | Tujuan |
|---|---|
| Account Logon | Kerberos/credential validation di DC |
| Account Management | perubahan user/group/computer |
| Directory Service Access | perubahan object AD jika SACL ada |
| Logon/Logoff | logon ke DC/server |
| Policy Change | perubahan audit/trust/policy |
| Privilege Use | penggunaan privilege sensitif |
| System | service/security state |

Event ID penting:

| Event ID | Arti |
|---:|---|
| 4624 | logon sukses |
| 4625 | logon gagal |
| 4648 | explicit credentials |
| 4672 | special privileges assigned |
| 4688 | process creation |
| 4719 | audit policy changed |
| 4720 | user created |
| 4722 | user enabled |
| 4723 | password change attempt |
| 4724 | password reset attempt |
| 4725 | user disabled |
| 4726 | user deleted |
| 4728 | member added to global group |
| 4732 | member added to local group |
| 4738 | user changed |
| 4740 | account locked out |
| 4741 | computer account created |
| 4742 | computer account changed |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos pre-auth failed |
| 4776 | NTLM credential validation |
| 5136 | directory object modified |
| 5137 | directory object created |
| 5141 | directory object deleted |
| 7045 | service installed |
| 1102 | audit log cleared |

```powershell
# Tampilkan audit policy DC.
auditpol /get /category:*

# Cari account lockout events.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4740} -MaxEvents 20

# Cari group membership changes penting.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4728,4732,4756} -MaxEvents 50

# Cari Kerberos pre-auth failure.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4771} -MaxEvents 50

# Cari audit log cleared.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=1102} -MaxEvents 10
```

### 8.6 Common AD Attack Paths as Defensive Knowledge

Security engineer perlu memahami attack path AD agar bisa menutup jalur, bukan untuk menjalankan serangan di environment yang tidak sah.

Pola risiko umum:

| Risiko | Defense |
|---|---|
| password reuse admin lokal | LAPS/Windows LAPS |
| Domain Admin login ke workstation | tiering dan PAW |
| service account password lemah | gMSA dan password panjang |
| excessive delegation | review ACL dan delegation |
| unconstrained delegation | hapus/ganti constrained delegation |
| stale privileged membership | periodic access review |
| weak Kerberos encryption | disable legacy jika kompatibel |
| NTLM relay exposure | SMB/LDAP signing, EPA/channel binding |
| AD CS misconfiguration | audit template dan enrollment rights |
| GPO abuse | protect GPO edit rights |
| DNSAdmins abuse risk | batasi membership |
| backup operator data access | batasi dan monitor |

Defensive inventory:

```powershell
# Cari privileged group members.
"Domain Admins","Enterprise Admins","Schema Admins","Administrators" | ForEach-Object {
    Get-ADGroupMember $_ -Recursive | Select-Object @{Name="Group";Expression={$_}}, Name, SamAccountName, objectClass
}

# Cari enabled user yang password never expires.
Get-ADUser -Filter {Enabled -eq $true -and PasswordNeverExpires -eq $true} -Properties PasswordNeverExpires | Select-Object SamAccountName, PasswordNeverExpires

# Cari inactive computers lebih dari 90 hari.
Search-ADAccount -AccountInactive -ComputersOnly -TimeSpan 90.00:00:00

# Cari accounts yang trusted for delegation.
Get-ADObject -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=524288)" -Properties servicePrincipalName
```

---

## 9.0 Backup, Restore, and Recovery

### 9.1 System State Backup

Domain controller backup harus mencakup System State. Untuk DC, System State mencakup AD database, SYSVOL, registry, boot files, COM+ class registration, dan komponen penting lain.

Prinsip backup DC:

- backup minimal dua DC jika memungkinkan
- simpan backup aman dan offline/immutable jika bisa
- test restore di lab terisolasi
- jangan hanya mengandalkan VM snapshot
- dokumentasikan Directory Services Restore Mode password
- lindungi backup DC seperti melindungi DC

```powershell
# Install Windows Server Backup.
Install-WindowsFeature Windows-Server-Backup

# Cek VSS writers.
vssadmin list writers

# Mulai system state backup ke volume backup.
wbadmin start systemstatebackup -backuptarget:E: -quiet

# Tampilkan backup yang tersedia.
wbadmin get versions
```

### 9.2 AD Recycle Bin and Object Restore

AD Recycle Bin memungkinkan restore object deleted beserta banyak attribute penting. Fitur ini harus diaktifkan di forest dan tidak bisa dimatikan setelah aktif.

Sebelum AD Recycle Bin, restore object deleted lebih terbatas karena banyak attribute hilang saat tombstone. Dengan Recycle Bin, object masuk state deleted lalu recycled setelah deleted object lifetime.

```powershell
# Cek apakah AD Recycle Bin aktif.
Get-ADOptionalFeature -Filter 'Name -like "Recycle Bin Feature"'

# Aktifkan AD Recycle Bin untuk forest lab.
Enable-ADOptionalFeature "Recycle Bin Feature" -Scope ForestOrConfigurationSet -Target lab.local

# Cari deleted objects.
Get-ADObject -Filter 'isDeleted -eq $true -and Name -like "*Alice*"' -IncludeDeletedObjects

# Restore deleted object berdasarkan identity.
Restore-ADObject -Identity "object-guid-here"
```

### 9.3 Authoritative and Non-Authoritative Restore

Non-authoritative restore mengembalikan DC dari backup lalu membiarkan replication partner memperbarui DC dengan data terbaru. Ini umum untuk memulihkan DC yang rusak.

Authoritative restore dipakai ketika object/OU yang terhapus perlu dikembalikan dan dibuat lebih baru secara replication metadata agar replicate ke DC lain. Ini operasi sensitif dan biasanya dilakukan melalui DSRM dengan `ntdsutil`.

Perbedaan:

| Restore | Fungsi |
|---|---|
| non-authoritative | DC restore lalu update dari partner |
| authoritative | object tertentu dibuat authoritative agar kembali replicate |
| bare metal | restore seluruh server |
| forest recovery | rebuild forest/domain dari kondisi besar |

Gunakan AD Recycle Bin untuk object restore jika tersedia sebelum memilih authoritative restore.

```powershell
# Restart ke Directory Services Restore Mode dari command line.
bcdedit /set safeboot dsrepair

# Setelah recovery selesai, hapus safeboot agar boot normal.
bcdedit /deletevalue safeboot

# Tampilkan bantuan ntdsutil.
ntdsutil
```

### 9.4 Forest Recovery Overview

Forest recovery adalah skenario besar ketika seluruh forest rusak/compromised/corrupt. Ini bukan operasi harian. Forest recovery harus punya runbook dan latihan.

Penyebab forest recovery:

| Penyebab | Contoh |
|---|---|
| compromise besar | attacker menguasai Tier 0 |
| corruption | database/replication rusak luas |
| accidental destructive change | delete massal tanpa restore mudah |
| ransomware | DC dan backup terdampak |

Prinsip:

- isolasi environment
- tentukan DC recovery source yang terpercaya
- restore forest root domain dulu
- seize/transfer FSMO sesuai prosedur
- clean metadata DC yang tidak dipulihkan
- reset krbtgt sesuai prosedur setelah compromise
- validasi DNS, SYSVOL, replication, trust, GPO

```powershell
# Tampilkan FSMO holders untuk dokumentasi recovery.
netdom query fsmo

# Export daftar domain controller.
Get-ADDomainController -Filter * | Select-Object HostName, Site, IsGlobalCatalog, OperationMasterRoles | Export-Csv C:\Temp\domain-controllers.csv -NoTypeInformation

# Export trust untuk dokumentasi recovery.
Get-ADTrust -Filter * | Export-Csv C:\Temp\ad-trusts.csv -NoTypeInformation
```

### 9.5 Disaster Recovery Runbook

Runbook DR AD harus ditulis sebelum incident. Saat AD down, banyak tool dan akses bisa ikut gagal.

Runbook minimum:

| Tahap | Isi |
|---|---|
| Contacts | owner AD, security, network, virtualization, backup |
| Access | emergency credentials dan DSRM password |
| Inventory | DC, FSMO, GC, DNS, sites, subnets |
| Backup | lokasi backup dan cara restore |
| Isolation | cara memutus DC compromised |
| Recovery order | forest root, DNS, GC, additional DC |
| Validation | dcdiag, repadmin, DNS SRV, SYSVOL |
| Security | reset credential, krbtgt plan, audit |
| Communication | stakeholder dan maintenance notice |

```powershell
# Export ringkasan AD untuk dokumentasi berkala.
Get-ADForest | Format-List * | Out-File C:\Temp\forest-info.txt

# Export informasi domain untuk dokumentasi berkala.
Get-ADDomain | Format-List * | Out-File C:\Temp\domain-info.txt

# Export site dan subnet.
Get-ADReplicationSite -Filter * | Select-Object Name | Export-Csv C:\Temp\ad-sites.csv -NoTypeInformation

# Export subnet AD.
Get-ADReplicationSubnet -Filter * | Select-Object Name, Site | Export-Csv C:\Temp\ad-subnets.csv -NoTypeInformation
```

---

## 10.0 Troubleshooting Playbooks

### 10.1 User Cannot Log In

Login failure bisa disebabkan password salah, account disabled, expired, locked, time skew, DNS, DC unreachable, workstation secure channel rusak, atau policy.

Urutan cek:

1. Pastikan error message.
2. Cek user status di AD.
3. Cek lockout/password expired.
4. Cek DC yang dipakai client.
5. Cek DNS dan time client.
6. Cek event log DC dan client.
7. Cek apakah hanya satu user atau banyak user.

```powershell
# Cek status user.
Get-ADUser alice -Properties Enabled, LockedOut, AccountExpirationDate, PasswordExpired, PasswordLastSet

# Unlock account jika terkunci.
Unlock-ADAccount alice

# Cek domain controller yang dipakai client dari CMD.
echo %LOGONSERVER%

# Cek secure channel client.
Test-ComputerSecureChannel

# Cari logon failure pada DC.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4625} -MaxEvents 50
```

### 10.2 Account Lockout

Account lockout sering berasal dari password lama yang tersimpan di service, scheduled task, mapped drive, mobile device, Wi-Fi/VPN, RDP session, atau aplikasi.

Data penting:

| Data | Sumber |
|---|---|
| user terkunci | AD attribute |
| waktu lockout | event 4740 |
| caller computer | event 4740 |
| DC yang mencatat | biasanya PDC atau DC auth |
| source logon | Security log client/server |

```powershell
# Cari event account lockout.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4740} -MaxEvents 20

# Cek user lockout attributes.
Get-ADUser alice -Properties LockedOut, lockoutTime, badPwdCount, badPasswordTime

# Cari DC PDC Emulator.
Get-ADDomain | Select-Object PDCEmulator

# Unlock user setelah source ditemukan atau mitigasi sementara.
Unlock-ADAccount alice
```

### 10.3 Domain Join Fails

Domain join bergantung pada DNS, network, time, credential, dan computer object. Error domain join sering sebenarnya DNS.

Checklist:

| Cek | Command |
|---|---|
| DNS client internal | `Get-DnsClientServerAddress` |
| resolve domain | `Resolve-DnsName lab.local` |
| SRV DC | `_ldap._tcp.dc._msdcs` |
| time | `w32tm /query /status` |
| port DC | `Test-NetConnection` |
| existing object | `Get-ADComputer` |

```powershell
# Cek DNS client server.
Get-DnsClientServerAddress

# Resolve domain.
Resolve-DnsName lab.local

# Resolve DC locator SRV.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Test LDAP port ke DC.
Test-NetConnection LAB-DC-01 -Port 389

# Join domain.
Add-Computer -DomainName lab.local -Restart
```

### 10.4 GPO Not Applying

GPO failure harus dicek dari client dan dari AD/SYSVOL.

Urutan:

1. Cek OU object.
2. Cek link GPO.
3. Cek security filtering.
4. Cek WMI filter.
5. Cek inheritance/enforced/block.
6. Cek SYSVOL.
7. Cek event GroupPolicy operational.
8. Cek replication.

```powershell
# Force policy update.
gpupdate /force

# Export gpresult.
gpresult /h C:\Temp\gpresult.html

# Cek GPO inheritance OU.
Get-GPInheritance -Target "OU=Servers,DC=lab,DC=local"

# Cek event Group Policy operational.
Get-WinEvent -LogName "Microsoft-Windows-GroupPolicy/Operational" -MaxEvents 50

# Cek akses SYSVOL.
dir \\lab.local\SYSVOL\lab.local\Policies
```

### 10.5 Replication Broken

Replication broken bisa berdampak ke user, password, group, GPO, DNS, dan DC locator. Jangan hanya melihat satu DC.

Urutan:

1. `repadmin /replsummary`
2. `repadmin /showrepl`
3. cek DNS record DC
4. cek RPC/network/firewall
5. cek time skew
6. cek Directory Service log
7. cek DFSR jika SYSVOL/GPO terdampak

```powershell
# Ringkas replication.
repadmin /replsummary

# Detail replication semua DC.
repadmin /showrepl *

# Cek failures via PowerShell.
Get-ADReplicationFailure -Target (Get-ADDomain).DNSRoot -Scope Domain

# Cek Directory Service log.
Get-WinEvent -LogName "Directory Service" -MaxEvents 50

# Cek DNS DC record.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV
```

### 10.6 DNS and DC Locator Failure

DC locator adalah proses client menemukan domain controller. Ini memakai DNS SRV record, site/subnet mapping, dan Netlogon.

Gejala:

| Gejala | Kemungkinan |
|---|---|
| login lambat | client memilih DC jauh |
| domain join gagal | DNS client salah/SRV hilang |
| GPO lambat | SYSVOL dari DC jauh |
| intermittent auth | SRV record stale atau DC unhealthy |

```powershell
# Cek DC locator dari client.
nltest /dsgetdc:lab.local

# Cek DC locator dengan force rediscovery.
nltest /dsgetdc:lab.local /force

# Cek site client.
nltest /dsgetsite

# Query SRV berdasarkan site.
Resolve-DnsName _ldap._tcp.Jakarta._sites.dc._msdcs.lab.local -Type SRV

# Restart Netlogon untuk refresh DC locator cache.
Restart-Service Netlogon
```

---

## Lab Checklist

Lab yang sebaiknya dibuat:

| Lab | Target |
|---|---|
| Build forest baru | memahami first DC, DNS, domain |
| Add second DC | memahami replication dan GC |
| AD-integrated DNS | membuat zone, SRV check, forwarder |
| OU design | membuat OU untuk users, servers, admins |
| Users/groups | membuat AGDLP permission model |
| Domain join | join client/member server |
| GPO baseline | membuat dan troubleshoot GPO |
| Loopback GPO | RDS/kiosk-like policy |
| Sites/subnets | client memilih DC sesuai site |
| Replication failure | membaca repadmin dan event |
| Account lockout | menemukan caller computer |
| gMSA | menjalankan service dengan managed password |
| AD Recycle Bin | delete dan restore object |
| Backup/restore | system state backup di lab |
| Security audit | review privileged groups dan dangerous delegation |

Mini scenario:

| Scenario | Yang Dilatih |
|---|---|
| user tidak bisa login | status user, lockout, DC, DNS, time |
| GPO tidak apply | scope, filter, SYSVOL, event |
| domain join gagal | DNS SRV, port, credential |
| file share access denied | token, group, ACL |
| password reset tidak berlaku | PDC, replication, DC choice |
| site branch login lambat | subnet mapping dan DC locator |
| duplicate SPN | Kerberos failure dan NTLM fallback |
| deleted OU | AD Recycle Bin dan restore |

---

## Command Index Cepat

Domain/forest:

```powershell
# Tampilkan informasi domain.
Get-ADDomain

# Tampilkan informasi forest.
Get-ADForest

# Tampilkan domain controller.
Get-ADDomainController -Filter *

# Tampilkan FSMO roles.
netdom query fsmo
```

DNS/DC:

```powershell
# Query DC locator SRV.
Resolve-DnsName _ldap._tcp.dc._msdcs.lab.local -Type SRV

# Cek DNS zones.
Get-DnsServerZone

# Diagnostic DC.
dcdiag

# Diagnostic DNS DC.
dcdiag /test:dns
```

Users/groups:

```powershell
# Cari user.
Get-ADUser alice -Properties *

# Cari group members.
Get-ADGroupMember "Domain Admins"

# Tambah user ke group.
Add-ADGroupMember -Identity "GG-Finance-Users" -Members alice

# Cari inactive user.
Search-ADAccount -AccountInactive -UsersOnly -TimeSpan 90.00:00:00
```

Kerberos/auth:

```powershell
# Tampilkan Kerberos tickets.
klist

# Purge Kerberos tickets.
klist purge

# Cari SPN.
setspn -Q HTTP/web01.lab.local

# Cari duplicate SPN.
setspn -X
```

GPO:

```powershell
# Tampilkan semua GPO.
Get-GPO -All

# Force update policy.
gpupdate /force

# Tampilkan result policy.
gpresult /r

# Export result policy HTML.
gpresult /h C:\Temp\gpresult.html
```

Replication:

```powershell
# Ringkas replication.
repadmin /replsummary

# Detail replication.
repadmin /showrepl

# Sync replication.
repadmin /syncall /AdeP

# Cek replication failures.
Get-ADReplicationFailure -Target (Get-ADDomain).DNSRoot -Scope Domain
```

Security:

```powershell
# Review privileged group members.
Get-ADGroupMember "Domain Admins"

# Cari password never expires.
Get-ADUser -Filter {PasswordNeverExpires -eq $true -and Enabled -eq $true} -Properties PasswordNeverExpires

# Cari account lockout events.
Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4740} -MaxEvents 20

# Cek audit policy.
auditpol /get /category:*
```

Recovery:

```powershell
# Backup system state.
wbadmin start systemstatebackup -backuptarget:E: -quiet

# Cek backup versions.
wbadmin get versions

# Cari deleted AD objects.
Get-ADObject -Filter 'isDeleted -eq $true' -IncludeDeletedObjects

# Restore deleted object.
Restore-ADObject -Identity "object-guid-here"
```

---

## Checklist Kesiapan Praktik

Kamu dianggap cukup siap mengelola dan mengamankan Active Directory jika bisa:

- menjelaskan forest, domain, tree, trust, OU, schema, GC, SID, RID, dan UPN
- menjelaskan kenapa DNS internal adalah dependency utama AD
- promote DC baru dan tahu cara demote DC dengan benar
- membaca health DC dengan `dcdiag`, `repadmin`, DNS SRV, SYSVOL, dan event log
- membuat user, group, computer, OU, dan service account dengan PowerShell
- memakai model AGDLP/AGUDLP untuk permission
- menjelaskan Kerberos TGT, service ticket, KDC, SPN, dan penyebab fallback NTLM
- troubleshoot login failure, account lockout, domain join, dan secure channel
- membuat, link, filter, dan troubleshoot GPO
- menjelaskan loopback processing dan kapan dipakai
- mendesain site/subnet agar client memilih DC terdekat
- menjelaskan replication partition, KCC, site link, dan replication failure
- menjelaskan 5 FSMO roles dan kapan transfer/seize
- menerapkan tiering model dan memisahkan admin account
- review privileged groups, dangerous rights, delegation, SPN, dan service accounts
- mengaktifkan audit penting dan membaca event ID AD/security utama
- melakukan backup system state dan memahami AD Recycle Bin
- tahu kapan perlu authoritative restore atau forest recovery
- siap lanjut ke security handbook yang lebih fokus ke defensive operations, detection engineering, dan incident response
