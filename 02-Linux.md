# Linux

Catatan Linux pribadi untuk belajar serius, praktik command, dan membangun fondasi administrasi Linux.

Fokus utama catatan ini:

- memahami konsep inti Linux
- mengingat command yang sering dipakai
- punya referensi cepat saat troubleshooting
- membangun kebiasaan administrasi sistem yang rapi dan aman

Area utama:

| Area | Cakupan |
|---|---|
| System Management | boot, filesystem, hardware, storage, network, shell, virtualization |
| Services and User Management | file, permission, user, group, process, package, systemd, container |
| Security | authentication, authorization, firewall, hardening, cryptography, audit |
| Automation, Orchestration, and Scripting | Bash, Python, Ansible, Git, CI/CD, AI-assisted workflow |
| Troubleshooting | monitoring, log, hardware, storage, OS, network, security, performance |

Gunakan halaman ini untuk memahami konsep, menjalankan command, mencatat observasi, dan mengulang praktik dari ingatan.

## Daftar Isi

- [0. Cara Pakai Catatan Ini](#0-cara-pakai-catatan-ini)
- [1.0 System Management](#10-system-management)
  - [1.1 Konsep Dasar Linux](#11-konsep-dasar-linux)
  - [1.2 Device Management](#12-device-management)
  - [1.3 Storage Management](#13-storage-management)
  - [1.4 Network Configuration](#14-network-configuration)
  - [1.5 Shell Operations](#15-shell-operations)
  - [1.6 Virtualization](#16-virtualization)
- [2.0 Services and User Management](#20-services-and-user-management)
  - [2.1 Files, Directories, Links, Permissions](#21-files-directories-links-permissions)
  - [2.2 Account Management](#22-account-management)
  - [2.3 Processes, Jobs, and Scheduling](#23-processes-jobs-and-scheduling)
  - [2.4 Software Management](#24-software-management)
  - [2.5 Systems Management with systemd](#25-systems-management-with-systemd)
  - [2.6 Containers](#26-containers)
- [3.0 Security](#30-security)
  - [3.1 Authorization, Authentication, Accounting](#31-authorization-authentication-accounting)
  - [3.2 Firewalls](#32-firewalls)
  - [3.3 OS Hardening](#33-os-hardening)
  - [3.4 Account Security](#34-account-security)
  - [3.5 Cryptography](#35-cryptography)
  - [3.6 Compliance, Integrity, and Auditing](#36-compliance-integrity-and-auditing)
- [4.0 Automation, Orchestration, and Scripting](#40-automation-orchestration-and-scripting)
  - [4.1 Automation and Orchestration](#41-automation-and-orchestration)
  - [4.2 Shell Scripting](#42-shell-scripting)
  - [4.3 Python Basics for Linux Admins](#43-python-basics-for-linux-admins)
  - [4.4 Version Control with Git](#44-version-control-with-git)
  - [4.5 AI Best Practices](#45-ai-best-practices)
- [5.0 Troubleshooting](#50-troubleshooting)
  - [5.1 System Monitoring](#51-system-monitoring)
  - [5.2 Hardware, Storage, and OS Issues](#52-hardware-storage-and-os-issues)
  - [5.3 Networking Issues](#53-networking-issues)
  - [5.4 Security Issues](#54-security-issues)
  - [5.5 Performance Issues](#55-performance-issues)

---

## 0. Cara Pakai Catatan Ini

Saat belajar atau bekerja dengan Linux, jangan hanya menghafal command. Pastikan bisa menjelaskan:

- apa masalahnya
- command apa yang dipakai
- output apa yang dicari
- file konfigurasi apa yang relevan
- perubahan mana yang sementara dan mana yang permanen
- cara rollback atau recovery

Checklist praktik:

- Jalankan command di VM Linux.
- Coba di distro Debian/Ubuntu dan RHEL/Fedora bila memungkinkan.
- Biasakan membaca `man`, `--help`, log, dan file konfigurasi.
- Catat error yang muncul, bukan hanya command yang berhasil.

### Kedalaman Belajar yang Diharapkan

Untuk setiap topik, targetkan tiga level pemahaman:

1. Konsep: tahu fungsi fitur, komponen yang terlibat, dan risiko perubahan.
2. Operasional: bisa menjalankan command, membaca output, dan mengubah konfigurasi.
3. Troubleshooting: bisa mengisolasi penyebab masalah dari log, status service, permission, network path, atau state storage.

Masalah Linux sering berbentuk skenario, bukan satu command tunggal. Contohnya, service gagal start bisa menyentuh `systemctl`, `journalctl`, permission file, SELinux context, port conflict, package dependency, dan firewall sekaligus.

### Cara Membaca Blok Command

Setiap command di catatan ini diberi komentar singkat tepat di atasnya. Komentar menjelaskan tujuan command, sedangkan command menunjukkan bentuk praktisnya.

Aturan membaca:

- command yang diawali `sudo` mengubah sistem atau membutuhkan privilege tinggi
- command inspeksi seperti `lsblk`, `ip addr`, dan `journalctl` dipakai sebelum melakukan perubahan
- command permanen biasanya menyentuh file konfigurasi, package database, firewall permanent rule, systemd enable, atau `/etc/fstab`
- command destructive seperti remove, delete, format, dan overwrite harus dicoba di VM lab lebih dulu

### Lab Minimum

Pola belajar di file ini:

- baca konsep singkat, lalu langsung jalankan command yang ada di section tersebut
- sebelum melihat output atau membaca penjelasan lanjutan, prediksi dulu apa yang seharusnya terjadi
- setelah command selesai, tulis observasi: output penting, gejala, layer/komponen yang terlibat, dan dugaan penyebab
- ulangi praktik yang sama di hari berikutnya tanpa melihat catatan agar ingatan dibangun lewat recall
- jalankan command hanya di sistem milik sendiri, lab pribadi, atau environment yang memang kamu diberi izin

---

## 1.0 System Management

**Praktik setelah bab ini:** kenali sistem dari kernel, storage, filesystem, process, dan service.

```bash
# Melihat kernel dan arsitektur sistem.
uname -a

# Melihat block device dan mountpoint.
lsblk -f

# Melihat filesystem yang sedang ter-mount.
findmnt

# Melihat process dengan penggunaan resource terbesar.
ps aux --sort=-%mem | head
```

Catat: kernel version, disk layout, filesystem type, mount option, process penting, dan service yang menjadi dependency sistem.

Bagian ini merangkum konsep Linux dasar, boot, hardware, storage, network, shell, backup, dan virtualization.

### 1.1 Konsep Dasar Linux

#### Boot Process

Urutan umum boot:

1. Firmware: `BIOS` atau `UEFI`
2. Bootloader: biasanya `GRUB`
3. Kernel Linux dimuat ke memory
4. `initrd` atau `initramfs` menyiapkan environment awal
5. Init system berjalan, umumnya `systemd`
6. Service dan target default dijalankan

Penjelasan:

- Firmware adalah komponen pertama yang berjalan setelah mesin dinyalakan. Di sistem lama biasanya `BIOS`, sedangkan sistem modern umumnya memakai `UEFI`.
- Bootloader bertugas menemukan kernel, membaca konfigurasi boot, dan meneruskan parameter ke kernel.
- Kernel mulai mengambil alih hardware, memory, process scheduling, storage driver, network driver, dan filesystem dasar.
- `initramfs` penting ketika root filesystem membutuhkan driver khusus, LVM, RAID, atau encryption sebelum bisa di-mount.
- Setelah root filesystem siap, `systemd` menjalankan unit-unit sesuai target default.
- Jika boot gagal, biasanya titik masalah berada di GRUB, kernel parameter, initramfs, `/etc/fstab`, driver storage, atau service yang blocking saat startup.

Komponen penting:

- `GRUB`: bootloader, memilih kernel dan parameter boot
- kernel parameters: opsi seperti `ro`, `quiet`, `single`, `systemd.unit=rescue.target`
- `initrd`/`initramfs`: temporary root filesystem saat boot awal
- `PXE`: boot lewat network, sering dipakai provisioning server

Command boot dan systemd:

```bash
# Menampilkan informasi kernel dan arsitektur sistem.
uname -a

# Membaca isi file langsung ke terminal.
cat /proc/cmdline

# Mengelola atau memeriksa unit dan service systemd.
systemctl get-default

# Mengelola atau memeriksa unit dan service systemd.
systemctl set-default multi-user.target

# Mengelola atau memeriksa unit dan service systemd.
systemctl isolate rescue.target

# Mengelola atau memeriksa unit dan service systemd.
systemctl list-units --failed

# Menganalisis performa dan urutan boot systemd.
systemd-analyze

# Menganalisis performa dan urutan boot systemd.
systemd-analyze blame

# Membaca log dari systemd journal.
journalctl -b

# Membaca log dari systemd journal.
journalctl -u sshd
```

File penting:

```text
/boot
/boot/grub
/etc/default/grub
/etc/fstab
/etc/systemd/system
/usr/lib/systemd/system
```

Cara membaca file/lokasi ini:

- `/boot` harus cukup ruang karena berisi kernel dan image initramfs. Jika penuh, update kernel bisa gagal.
- `/etc/default/grub` biasanya dipakai untuk mengubah default kernel parameter, timeout, atau opsi menu GRUB.
- `/etc/fstab` sangat sensitif saat boot. Entry yang salah bisa membuat sistem masuk emergency mode.
- `/etc/systemd/system` berisi unit lokal atau override yang biasanya lebih prioritas dari unit bawaan paket.
- `/usr/lib/systemd/system` atau `/lib/systemd/system` berisi unit bawaan package. Lebih baik pakai override daripada mengedit file bawaan langsung.

Update konfigurasi GRUB:

```bash
# Membangun ulang konfigurasi GRUB setelah perubahan bootloader.
sudo grub2-mkconfig -o /boot/grub2/grub.cfg

# Membangun ulang konfigurasi GRUB setelah perubahan bootloader.
sudo update-grub
```

Catatan:

- `update-grub` umum di Debian/Ubuntu.
- `grub2-mkconfig` umum di RHEL/Fedora.
- Jangan ubah parameter boot permanen tanpa tahu cara rollback.

#### Filesystem Hierarchy Standard

Direktori penting:

| Path | Fungsi |
|---|---|
| `/` | root filesystem |
| `/bin` | command penting untuk user |
| `/sbin` | command administrasi sistem |
| `/boot` | kernel, initramfs, bootloader files |
| `/dev` | device files |
| `/etc` | konfigurasi sistem |
| `/home` | home directory user biasa |
| `/root` | home directory root |
| `/lib`, `/lib64` | library penting |
| `/proc` | virtual filesystem untuk process/kernel |
| `/sys` | virtual filesystem untuk device/kernel |
| `/run` | runtime state |
| `/tmp` | temporary files |
| `/usr` | aplikasi, library, dokumentasi |
| `/usr/local` | software lokal/manual |
| `/var` | log, spool, cache, data berubah |
| `/mnt` | mount manual sementara |
| `/media` | removable media |
| `/opt` | optional third-party software |
| `/srv` | data service |

Penjelasan:

- Linux menaruh konfigurasi dan data di lokasi yang relatif konsisten agar administrasi sistem lebih mudah diprediksi.
- `/etc` biasanya menjadi tempat pertama saat mencari konfigurasi service.
- `/var` sering menjadi sumber masalah disk penuh karena log, cache, spool mail, database, atau file upload aplikasi.
- `/proc` dan `/sys` bukan file biasa di disk. Keduanya adalah tampilan runtime dari kernel, process, dan device.
- `/run` bersifat sementara sejak boot terakhir, sehingga jangan menyimpan konfigurasi permanen di sana.
- `/tmp` bisa dibersihkan otomatis oleh sistem, jadi jangan simpan data penting jangka panjang di sana.
- Pada distro modern, `/bin`, `/sbin`, dan `/lib` sering menjadi symlink ke lokasi di bawah `/usr`.

usrmerge:

- `usrmerge` adalah pola modern yang menggabungkan direktori lama seperti `/bin`, `/sbin`, `/lib`, dan `/lib64` menjadi symlink ke `/usr/bin`, `/usr/sbin`, `/usr/lib`, dan `/usr/lib64`.
- Dulu direktori tersebut dipisah karena `/usr` bisa berada di filesystem terpisah dan belum tentu tersedia pada boot awal.
- Pada sistem modern, initramfs membuat kebutuhan boot awal berbeda, sehingga pemisahan itu makin jarang dibutuhkan.
- Efek praktisnya: command seperti `/bin/ls` dan `/usr/bin/ls` bisa menunjuk file yang sama.
- Saat membuat script, lebih portable memakai `/usr/bin/env bash` untuk mencari interpreter dari `PATH`.

Command inspeksi:

```bash
# Menampilkan isi direktori atau file yang cocok.
ls /

# Menampilkan struktur direktori dalam bentuk pohon.
tree -L 1 /

# Menampilkan atau mengelola mount filesystem.
findmnt

# Menampilkan atau mengelola mount filesystem.
mount

# Menampilkan penggunaan kapasitas filesystem.
df -h

# Menghitung ukuran file atau direktori.
du -sh /var/*
```

#### Arsitektur Server

Arsitektur yang perlu dikenali:

- `x86`
- `x86_64` atau `AMD64`
- `AArch64`
- `RISC-V`

Penjelasan:

- Arsitektur menentukan binary/package mana yang bisa dijalankan. Package untuk `x86_64` tidak otomatis bisa berjalan di `AArch64`.
- `x86_64` adalah arsitektur paling umum untuk server dan laptop modern.
- `AArch64` umum di cloud instance ARM, Raspberry Pi 64-bit, dan beberapa server hemat daya.
- `RISC-V` mulai muncul di hardware eksperimen dan embedded, tetapi belum seumum `x86_64` atau ARM.
- Saat troubleshooting package atau container image yang gagal jalan, cek arsitektur host dan arsitektur image/package.

Command:

```bash
# Menampilkan informasi kernel dan arsitektur sistem.
uname -m

# Menampilkan detail CPU dan arsitektur mesin.
lscpu

# Membaca isi file langsung ke terminal.
cat /proc/cpuinfo
```

#### Distribusi Linux

Keluarga umum:

- Debian-based: Debian, Ubuntu, Linux Mint
- RPM-based: RHEL, Fedora, CentOS Stream, Rocky Linux, AlmaLinux, openSUSE

Package format:

- Debian: `.deb`, dikelola `dpkg`, `apt`
- RPM: `.rpm`, dikelola `rpm`, `dnf`, `yum`, `zypper`

Penjelasan:

- Perbedaan distro paling terasa pada package manager, lokasi konfigurasi tertentu, nama service, dan default security policy.
- Debian/Ubuntu sering memakai `apt`, sedangkan RHEL/Fedora/Rocky/Alma memakai `dnf` atau `yum`.
- Command tingkat tinggi seperti `apt` dan `dnf` mengurus dependency dan repository.
- Command tingkat rendah seperti `dpkg` dan `rpm` berguna untuk inspeksi detail, tetapi bisa meninggalkan dependency belum selesai jika dipakai sembarangan.
- Saat membaca dokumentasi, selalu cocokkan instruksi dengan keluarga distro yang dipakai.

Command:

```bash
# Membaca isi file langsung ke terminal.
cat /etc/os-release

# Menampilkan informasi distribusi Linux.
lsb_release -a

# Mengelola package dan repository pada distro Debian/Ubuntu.
apt list --installed

# Mengelola package dan repository pada distro RPM-based.
dnf list installed

# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -qa

# Mengelola package DEB tingkat rendah.
dpkg -l
```

#### GUI Linux

Komponen GUI:

- display manager: `gdm`, `sddm`, `lightdm`
- window manager: mengatur window
- desktop environment: GNOME, KDE Plasma, XFCE
- `X Server`: display server lama/tradisional
- `Wayland`: display server modern

Penjelasan:

- Server Linux sering berjalan tanpa GUI, tetapi workstation dan beberapa server admin tool memakai desktop environment.
- Display manager menangani layar login grafis.
- Desktop environment menyediakan panel, settings, file manager, dan aplikasi grafis bawaan.
- Window manager mengatur posisi, ukuran, dan fokus window.
- `Wayland` lebih modern dan aman dalam desain isolasi input/output, tetapi beberapa tool remote desktop atau screen sharing lama masih lebih mudah di `X Server`.

Command:

```bash
# Mencetak nilai atau teks ke terminal.
echo $XDG_SESSION_TYPE

# Mengelola atau memeriksa unit dan service systemd.
systemctl status gdm

# Melihat sesi login dan seat yang dikelola systemd.
loginctl
```

#### Software Licensing

Konsep lisensi:

- open source software: source code tersedia sesuai lisensi
- free software: menekankan kebebasan menggunakan, mempelajari, mengubah, membagikan
- proprietary software: source code tidak bebas
- copyleft: turunan software harus mempertahankan kebebasan lisensi

Penjelasan:

- Lisensi memengaruhi apa yang boleh dilakukan dengan software, terutama saat mendistribusikan ulang binary, container image, atau appliance.
- Lisensi permissive seperti MIT/BSD/Apache biasanya memberi kebebasan besar untuk memakai dan mendistribusikan software.
- Lisensi copyleft seperti GPL bisa mewajibkan source code turunan tetap tersedia dengan lisensi kompatibel.
- Untuk kerja profesional, jangan hanya cek apakah software gratis dipakai. Cek juga hak distribusi, attribution, dan kewajiban source disclosure.

Contoh:

- GPL: copyleft kuat
- LGPL: copyleft lebih longgar untuk library
- MIT/BSD/Apache: permissive

### 1.2 Device Management

#### Kernel Modules

Kernel module adalah driver atau fitur yang bisa dimuat/dilepas dari kernel.

Penjelasan:

- Kernel module membuat kernel bisa mendukung hardware atau fitur baru tanpa rebuild kernel penuh.
- Contoh module umum adalah driver network card, storage controller, filesystem, virtualization, dan security feature.
- `modprobe` lebih aman dipakai daripada `insmod` karena memahami dependency module.
- Melepas module yang sedang dipakai bisa gagal atau mengganggu service, jadi cek pemakaian dan log kernel lebih dulu.
- Konfigurasi permanen module biasanya diletakkan di `/etc/modules-load.d/` atau `/etc/modprobe.d/`.

Command penting:

```bash
# Menampilkan kernel module yang sedang dimuat.
lsmod

# Melihat metadata dan parameter kernel module.
modinfo <module>

# Memuat atau melepas kernel module beserta dependency.
sudo modprobe <module>

# Memuat atau melepas kernel module beserta dependency.
sudo modprobe -r <module>

# Memuat file kernel module secara langsung.
sudo insmod ./module.ko

# Melepas kernel module dari kernel.
sudo rmmod <module>

# Membangun ulang dependency database kernel module.
sudo depmod
```

File penting:

```text
/lib/modules/$(uname -r)
/etc/modules-load.d/
/etc/modprobe.d/
```

Troubleshooting module:

```bash
# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | tail

# Membaca log dari systemd journal.
journalctl -k

# Melihat metadata dan parameter kernel module.
modinfo e1000e

# Menampilkan kernel module yang sedang dimuat.
lsmod | grep e1000e
```

#### Hardware Inspection

Hardware inspection dipakai untuk memahami perangkat apa saja yang dikenali kernel dan bagaimana kondisinya.

Sebelum menjalankan command, pahami alurnya:

- CPU dan memory menjelaskan kapasitas compute dasar.
- PCI/USB menjelaskan device yang terpasang.
- Kernel log menjelaskan apakah device berhasil dikenali atau ada error driver.
- DMI/SMBIOS menjelaskan informasi dari firmware seperti vendor, model, serial, dan slot RAM.
- Sensor dan IPMI membantu melihat kondisi fisik server seperti suhu, fan, power, dan event hardware.

Command hardware:

```bash
# Menampilkan detail CPU dan arsitektur mesin.
lscpu

# Menampilkan informasi blok memory sistem.
lsmem

# Menampilkan perangkat PCI yang terdeteksi.
lspci

# Menampilkan perangkat USB yang terdeteksi.
lsusb

# Menampilkan struktur block device, partition, dan mount point.
lsblk

# Menampilkan inventaris hardware secara detail.
lshw

# Membaca informasi hardware dari tabel DMI/SMBIOS.
dmidecode

# Membaca pesan kernel, terutama error hardware dan driver.
dmesg

# Mengakses manajemen hardware server melalui IPMI.
ipmitool

# Membaca sensor hardware seperti suhu dan fan.
sensors
```

Package/tool:

- `lm_sensors`: membaca sensor temperature/fan/voltage
- `ipmitool`: manajemen server out-of-band lewat IPMI
- `nvtop`: monitoring GPU

Penjelasan:

- Hardware inspection dipakai saat validasi server baru, debugging driver, cek kapasitas RAM/CPU, atau mencari device yang tidak terdeteksi.
- `lspci` berguna untuk device internal seperti NIC, storage controller, GPU, dan chipset.
- `lsusb` berguna untuk device USB seperti dongle, storage external, UPS, atau serial adapter.
- `dmesg` sering menunjukkan error driver, firmware missing, disk reset, USB disconnect, atau NIC link issue.
- `dmidecode` membaca informasi dari firmware, seperti model server, serial number, slot RAM, dan BIOS version.
- Pada server fisik, `ipmitool` bisa membantu membaca sensor, power state, dan event log tanpa bergantung penuh pada OS.

Contoh:

```bash
# Membaca informasi hardware dari tabel DMI/SMBIOS.
sudo dmidecode -t system

# Menampilkan inventaris hardware secara detail.
sudo lshw -short

# Menampilkan perangkat PCI yang terdeteksi.
lspci -nn

# Menampilkan perangkat USB yang terdeteksi.
lsusb

# Membaca sensor hardware seperti suhu dan fan.
sensors
```

#### initrd Management

Tool:

- `dracut`: umum di RHEL/Fedora
- `mkinitrd`: tool lama atau distro tertentu
- `update-initramfs`: umum di Debian/Ubuntu

Penjelasan:

- `initrd` atau `initramfs` adalah environment kecil yang dipakai sebelum root filesystem utama siap.
- Jika root filesystem ada di LVM, RAID, encrypted disk, atau storage controller tertentu, initramfs harus punya module dan tool yang tepat.
- Setelah mengubah driver storage, kernel module penting, LUKS, atau layout root filesystem, rebuild initramfs sering diperlukan.
- Jika sistem gagal boot setelah perubahan storage, salah satu dugaan utama adalah initramfs tidak berisi komponen yang dibutuhkan.

Command:

```bash
# Membangun ulang initramfs/initrd untuk proses boot.
sudo dracut -f

# Membangun ulang initramfs/initrd untuk proses boot.
sudo update-initramfs -u

# Melihat isi image initramfs/initrd.
lsinitrd

# Melihat isi image initramfs/initrd.
lsinitramfs /boot/initrd.img-$(uname -r)
```

Kapan dipakai:

- setelah menambah driver storage
- setelah perubahan LVM/encryption
- saat sistem gagal boot karena initramfs tidak punya module yang dibutuhkan

### 1.3 Storage Management

#### Device, Partition, Filesystem

Konsep:

- disk/device: `/dev/sda`, `/dev/nvme0n1`
- partition: `/dev/sda1`, `/dev/nvme0n1p1`
- filesystem: `ext4`, `xfs`, `btrfs`, `tmpfs`
- mount point: direktori tempat filesystem dipasang

Penjelasan:

- Block device adalah representasi kernel untuk disk atau volume storage.
- Partition membagi disk fisik/virtual menjadi area logis.
- Filesystem menentukan cara file dan metadata disimpan di atas partition atau volume.
- Mount point membuat filesystem bisa diakses lewat directory tree Linux.
- Jangan menjalankan `mkfs` pada device yang salah. `mkfs` membuat filesystem baru dan dapat menghapus data yang ada.
- Biasakan cek urutan `lsblk`, `blkid`, lalu baru melakukan perubahan.

Command inspeksi:

```bash
# Menampilkan struktur block device, partition, dan mount point.
lsblk

# Menampilkan UUID, label, dan tipe filesystem.
blkid

# Membuat atau memeriksa partition table disk.
fdisk -l

# Membuat atau memeriksa partition table disk.
parted -l

# Menampilkan atau mengelola mount filesystem.
findmnt

# Menampilkan penggunaan kapasitas filesystem.
df -h

# Menghitung ukuran file atau direktori.
du -sh .
```

Partition tools:

```bash
# Membuat atau memeriksa partition table disk.
sudo fdisk /dev/sdb

# Membuat atau memeriksa partition table disk.
sudo gdisk /dev/sdb

# Membuat atau memeriksa partition table disk.
sudo parted /dev/sdb

# Memperbesar partition, umum pada disk cloud atau VM.
sudo growpart /dev/sda 1
```

Catatan:

- `fdisk`: umum untuk MBR/GPT modern
- `gdisk`: fokus GPT
- `parted`: scriptable dan cocok untuk disk besar
- `growpart`: memperbesar partition, sering dipakai di cloud VM

#### Filesystems

Filesystem adalah format penyimpanan yang menentukan bagaimana file, directory, permission, timestamp, dan metadata diletakkan di atas block device.

Sebelum membuat filesystem:

- pastikan device benar dengan `lsblk` dan `blkid`
- pastikan tidak ada data penting di device tersebut
- pilih filesystem sesuai kebutuhan workload
- pahami apakah filesystem bisa grow, shrink, snapshot, atau repair online

Format umum:

```bash
# Membuat filesystem baru pada block device.
sudo mkfs.ext4 /dev/sdb1

# Membuat filesystem baru pada block device.
sudo mkfs.xfs /dev/sdb1

# Membuat filesystem baru pada block device.
sudo mkfs.btrfs /dev/sdb1
```

Repair dan resize:

```bash
# Memeriksa dan memperbaiki filesystem.
sudo fsck /dev/sdb1

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo resize2fs /dev/sdb1

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo xfs_growfs /mountpoint

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo xfs_repair /dev/sdb1
```

Catatan:

- `ext4` bisa grow dan shrink, tapi shrink harus hati-hati dan biasanya offline.
- `xfs` bisa grow online, tapi tidak shrink.
- `btrfs` punya snapshot/subvolume.
- `tmpfs` berada di memory/swap, cocok untuk temporary data.

Penjelasan:

- `ext4` adalah pilihan umum yang stabil dan mudah dipahami untuk banyak workload.
- `xfs` umum di server enterprise, terutama untuk filesystem besar dan workload yang butuh grow online.
- `btrfs` menarik untuk snapshot, checksum, dan subvolume, tetapi perlu pemahaman lebih matang saat recovery.
- `tmpfs` cepat karena memakai memory, tetapi data hilang saat reboot dan tetap bisa menekan memory sistem.
- Tool repair berbeda antar filesystem. Jangan memakai tool `ext4` untuk `xfs`, atau sebaliknya.
- Saat filesystem tiba-tiba read-only, cek `dmesg`, storage health, dan jalankan repair dari environment yang aman.

#### Mount dan fstab

Mount adalah proses memasang filesystem ke directory tree Linux. Tanpa mount, filesystem di disk tidak bisa diakses lewat path biasa.

Yang perlu dipahami:

- mount manual hanya berlaku sampai reboot
- `/etc/fstab` membuat mount menjadi permanen
- mount by UUID lebih stabil daripada mount by device name
- mount option bisa memengaruhi security dan behavior aplikasi
- entry `/etc/fstab` yang salah bisa membuat boot gagal atau masuk emergency mode

Mount manual:

```bash
# Membuat direktori baru.
sudo mkdir -p /data

# Menampilkan atau mengelola mount filesystem.
sudo mount /dev/sdb1 /data

# Melepas filesystem dari mount point.
sudo umount /data
```

Mount by UUID:

```bash
# Menampilkan UUID, label, dan tipe filesystem.
blkid

# Mengelola atau memakai privilege administratif secara terkontrol.
sudoedit /etc/fstab

# Menampilkan atau mengelola mount filesystem.
sudo mount -a

# Menampilkan atau mengelola mount filesystem.
findmnt --verify
```

Contoh `/etc/fstab`:

```text
UUID=xxxx-xxxx /data ext4 defaults,noatime 0 2
```

Field `/etc/fstab`:

| Field | Contoh | Arti |
|---|---|---|
| device/spec | `UUID=xxxx-xxxx` | device, UUID, LABEL, atau remote share |
| mount point | `/data` | directory tempat filesystem dipasang |
| filesystem type | `ext4` | tipe filesystem |
| options | `defaults,noatime` | opsi mount |
| dump | `0` | dipakai tool dump lama, biasanya `0` |
| fsck pass | `2` | urutan pengecekan filesystem saat boot |

Nilai `fsck pass` umum:

- `0`: jangan dicek otomatis
- `1`: root filesystem, dicek lebih dulu
- `2`: filesystem lain, dicek setelah root

File mount terkait:

```text
/etc/fstab
/etc/mtab
/proc/mounts
```

Mount options yang sering muncul:

- `defaults`
- `ro`
- `rw`
- `noatime`
- `nodev`
- `nosuid`
- `noexec`

Penjelasan:

- Mount manual hilang setelah reboot, sedangkan entry di `/etc/fstab` dipakai saat boot atau saat `mount -a`.
- Gunakan `UUID` atau `LABEL` untuk mount permanen agar tidak bergantung pada nama device seperti `/dev/sdb1` yang bisa berubah.
- `ro` membuat filesystem read-only, berguna untuk recovery atau proteksi data.
- `noexec` mencegah binary dieksekusi dari filesystem tersebut, berguna untuk `/tmp` atau area upload tertentu.
- `nodev` mencegah device file di filesystem diperlakukan sebagai device sungguhan.
- `nosuid` mengabaikan SUID/SGID bit, mengurangi risiko privilege escalation.
- Setelah mengubah `/etc/fstab`, selalu jalankan `mount -a` atau `findmnt --verify` sebelum reboot.

Bind mount:

- Bind mount memasang directory yang sudah ada ke lokasi lain.
- Ini bukan copy data; dua path menunjuk isi directory yang sama.
- Berguna untuk chroot, container, service isolation, atau membuat path kompatibel dengan aplikasi.

Contoh konsep:

```text
/srv/app-data  -> dipasang juga sebagai -> /var/lib/app/data
```

Remote filesystem:

| Jenis | Kegunaan |
|---|---|
| NFS | sharing filesystem antar Linux/Unix |
| SMB/CIFS | sharing dengan Windows/Samba |
| SSHFS | mount filesystem lewat SSH, cocok untuk penggunaan ringan |

Hal yang perlu diperhatikan pada remote filesystem:

- network down bisa membuat akses file hang atau lambat
- permission bisa dipengaruhi UID/GID mapping
- credential harus disimpan aman
- opsi timeout dan retry penting untuk server production

Removable media:

- biasanya muncul sebagai device baru di `lsblk`
- desktop environment sering melakukan automount
- server biasanya lebih aman melakukan mount manual
- selalu unmount sebelum mencabut storage agar data tidak korup

Read-only dan recovery:

- Filesystem bisa berubah read-only jika kernel mendeteksi error serius.
- Jangan langsung remount `rw` sebelum memahami penyebabnya.
- Cek `dmesg`, status disk, dan log kernel.
- Jalankan `fsck` pada filesystem yang tidak sedang mounted.
- Untuk root filesystem, gunakan rescue mode/live environment jika perlu.

Autofs:

```bash
# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl enable --now autofs

# Membaca isi file langsung ke terminal.
cat /etc/auto.master
```

`autofs` melakukan mount otomatis saat path diakses, sering untuk NFS/home directory.

#### LVM

Object LVM:

- PV: physical volume
- VG: volume group
- LV: logical volume

Penjelasan:

- LVM memberi lapisan fleksibel di atas disk/partition agar kapasitas bisa diperbesar, dipindah, atau dipecah menjadi beberapa logical volume.
- PV adalah storage mentah yang disiapkan untuk LVM.
- VG adalah pool kapasitas gabungan dari satu atau lebih PV.
- LV adalah volume yang dipakai aplikasi atau filesystem, mirip partition fleksibel.
- Extend LV relatif aman jika filesystem mendukung grow, tetapi reduce berisiko tinggi dan wajib backup.
- Snapshot LVM berguna untuk backup konsisten, tetapi snapshot yang penuh bisa invalid dan mengganggu workflow.

Workflow:

```bash
# Membuat object LVM untuk storage fleksibel.
sudo pvcreate /dev/sdb1

# Membuat object LVM untuk storage fleksibel.
sudo vgcreate vg_data /dev/sdb1

# Membuat object LVM untuk storage fleksibel.
sudo lvcreate -n lv_app -L 20G vg_data

# Membuat filesystem baru pada block device.
sudo mkfs.ext4 /dev/vg_data/lv_app

# Membuat direktori baru.
sudo mkdir -p /app

# Menampilkan atau mengelola mount filesystem.
sudo mount /dev/vg_data/lv_app /app
```

Inspect:

```bash
# Melihat status dan detail object LVM.
pvs

# Melihat status dan detail object LVM.
vgs

# Melihat status dan detail object LVM.
lvs

# Melihat status dan detail object LVM.
pvdisplay

# Melihat status dan detail object LVM.
vgdisplay

# Melihat status dan detail object LVM.
lvdisplay
```

Extend:

```bash
# Menambah kapasitas Volume Group atau Logical Volume.
sudo vgextend vg_data /dev/sdc1

# Menambah kapasitas Volume Group atau Logical Volume.
sudo lvextend -L +10G /dev/vg_data/lv_app

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo resize2fs /dev/vg_data/lv_app

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo xfs_growfs /app
```

Reduce `ext4` dengan hati-hati:

```bash
# Melepas filesystem dari mount point.
sudo umount /app

# Memeriksa lalu mengecilkan volume dengan sangat hati-hati.
sudo e2fsck -f /dev/vg_data/lv_app

# Mengubah ukuran atau memperbaiki filesystem tertentu.
sudo resize2fs /dev/vg_data/lv_app 10G

# Memeriksa lalu mengecilkan volume dengan sangat hati-hati.
sudo lvreduce -L 10G /dev/vg_data/lv_app
```

Perintah lain:

```bash
# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo lvchange -ay /dev/vg_data/lv_app

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo lvremove /dev/vg_data/lv_app

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo vgremove vg_data

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo pvremove /dev/sdb1

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo pvmove /dev/sdb1

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo vgscan

# Mengubah, menghapus, memindahkan, atau memindai object LVM.
sudo pvscan
```

#### RAID

Linux software RAID menggunakan `mdadm`.

Penjelasan:

- RAID meningkatkan redundancy, performa, atau keduanya, tetapi bukan pengganti backup.
- RAID 0 meningkatkan performa/kapasitas tetapi jika satu disk rusak seluruh array rusak.
- RAID 1 menyimpan mirror data ke dua disk atau lebih, cocok untuk redundancy sederhana.
- RAID 5/6 memakai parity dan butuh proses rebuild saat disk diganti.
- RAID 10 menggabungkan mirror dan stripe, sering lebih baik untuk performa dan rebuild time.
- Selalu monitor `/proc/mdstat` dan alert RAID degraded. Array degraded yang dibiarkan terlalu lama meningkatkan risiko kehilangan data.

Command:

```bash
# Membaca isi file langsung ke terminal.
cat /proc/mdstat

# Membuat, memeriksa, atau memperbaiki software RAID.
sudo mdadm --detail /dev/md0

# Membuat, memeriksa, atau memperbaiki software RAID.
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

# Membuat, memeriksa, atau memperbaiki software RAID.
sudo mdadm --fail /dev/md0 /dev/sdb

# Membuat, memeriksa, atau memperbaiki software RAID.
sudo mdadm --remove /dev/md0 /dev/sdb

# Membuat, memeriksa, atau memperbaiki software RAID.
sudo mdadm --add /dev/md0 /dev/sdd
```

Konsep:

- RAID 0: striping, cepat, tanpa redundancy
- RAID 1: mirroring
- RAID 5: parity, minimal 3 disk
- RAID 6: double parity
- RAID 10: mirror + stripe

File:

```text
/etc/mdadm.conf
/etc/mdadm/mdadm.conf
/proc/mdstat
```

#### Disk Usage, Inode, Quota

Bagian ini dipakai saat sistem tidak bisa menulis file, service gagal membuat log, package update gagal, atau aplikasi error karena storage.

Ada tiga jenis "penuh" yang berbeda:

- kapasitas byte penuh: ruang data habis
- inode penuh: jumlah file maksimum habis
- quota penuh: user/group/project dibatasi walaupun filesystem masih punya ruang

Command:

```bash
# Menampilkan penggunaan kapasitas filesystem.
df -h

# Menampilkan penggunaan kapasitas filesystem.
df -i

# Menghitung ukuran file atau direktori.
du -sh /var/*

# Mencari file berdasarkan nama, ukuran, tipe, atau kondisi lain.
find /var -xdev -type f -size +100M

# Melihat file atau socket yang sedang dibuka process.
lsof +L1

# Memeriksa penggunaan quota user atau filesystem.
quota -u user

# Memeriksa penggunaan quota user atau filesystem.
repquota -a
```

Masalah umum:

- disk full
- inode exhaustion
- file dihapus tapi masih dibuka process
- partition read-only karena filesystem error
- quota penuh

Penjelasan:

- Disk penuh tidak selalu berarti byte habis. Bisa juga inode habis karena terlalu banyak file kecil.
- `df -h` menjawab kapasitas byte, sedangkan `df -i` menjawab kapasitas inode.
- File yang sudah dihapus tetap memakai ruang jika masih dibuka process. Itu sebabnya `lsof +L1` penting.
- Directory seperti `/var/log`, `/var/cache`, `/tmp`, dan data aplikasi sering menjadi titik awal investigasi.
- Quota membatasi pemakaian user/group/project. Jika quota penuh, user bisa gagal menulis meskipun filesystem masih punya ruang.

#### Backup dan Restore

Backup adalah salinan data atau konfigurasi yang dibuat agar bisa dipulihkan saat terjadi kesalahan, kerusakan, atau kehilangan data.

Yang perlu dipahami sebelum memilih tool:

- archive cocok untuk mengemas banyak file menjadi satu file
- compression mengurangi ukuran archive
- sync cocok untuk menyalin perubahan berulang
- backup konfigurasi berbeda dari backup data aplikasi
- restore test lebih penting daripada sekadar sukses membuat file backup

Tools:

```bash
# Membuat atau mengekstrak archive.
tar

# Mengompresi file memakai format gzip.
gzip

# Mengompresi file memakai format bzip2.
bzip2

# Mengompresi file memakai format xz dengan rasio tinggi.
xz

# Mengompresi file memakai format zstd yang cepat dan modern.
zstd

# Menyalin dan menyinkronkan file secara efisien.
rsync

# Menyalin file antar host melalui SSH.
scp

# Mengelola transfer file interaktif melalui SSH.
sftp
```

Archive:

```bash
# Membuat atau mengekstrak archive.
tar -cvf backup.tar /etc

# Membuat atau mengekstrak archive.
tar -czvf backup.tar.gz /etc

# Membuat atau mengekstrak archive.
tar -cJvf backup.tar.xz /etc

# Membuat atau mengekstrak archive.
tar -xvf backup.tar
```

Rsync:

```bash
# Menyalin dan menyinkronkan file secara efisien.
rsync -avh /etc/ /backup/etc/

# Menyalin dan menyinkronkan file secara efisien.
rsync -avh --delete /src/ /dst/

# Menyalin dan menyinkronkan file secara efisien.
rsync -avh -e ssh /data/ user@server:/backup/data/
```

Prinsip restore:

- backup yang belum pernah dites restore belum bisa dipercaya
- simpan permission, owner, timestamp
- bedakan full, incremental, differential backup
- catat RPO dan RTO

Penjelasan:

- Backup melindungi dari human error, corruption, ransomware, disk failure, dan perubahan konfigurasi yang salah.
- `tar` cocok untuk archive lokal yang mempertahankan struktur directory.
- `rsync` cocok untuk sinkronisasi berulang karena hanya mengirim perubahan.
- `scp` dan `sftp` berguna untuk transfer sederhana lewat SSH.
- RPO menjawab seberapa banyak data boleh hilang, sedangkan RTO menjawab seberapa lama sistem boleh down.
- Restore test harus mencakup permission, owner, SELinux context bila relevan, service startup, dan integritas data aplikasi.

### 1.4 Network Configuration

#### Konsep Network

Hal yang harus dipahami:

- IP address
- CIDR prefix
- subnet
- default gateway
- DNS resolver
- hostname
- routing table
- listening socket
- firewall

Penjelasan:

- IP address adalah identitas host pada network tertentu.
- CIDR prefix seperti `/24` menentukan ukuran network dan host mana yang dianggap satu subnet.
- Default gateway dipakai untuk traffic ke luar subnet lokal.
- DNS mengubah nama seperti `example.com` menjadi IP address.
- Routing table menentukan lewat interface dan gateway mana paket dikirim.
- Listening socket menunjukkan service yang sedang menunggu koneksi.
- Firewall bisa memblokir traffic meskipun service sudah listen dan route benar.
- Troubleshooting network yang rapi selalu dimulai dari local interface, lalu route, DNS, service, firewall, dan baru remote path.

Konsep inti:

| Konsep | Contoh | Penjelasan |
|---|---|---|
| Interface | `eth0`, `ens33`, `wlan0` | device network yang dipakai host |
| IPv4 address | `192.168.1.10` | alamat host di network IPv4 |
| CIDR | `/24` | jumlah bit network prefix |
| Subnet | `192.168.1.0/24` | network tempat host berada |
| Gateway | `192.168.1.1` | router keluar subnet |
| DNS resolver | `1.1.1.1` | server yang menjawab query nama domain |
| Route | `default via 192.168.1.1` | aturan jalur packet |
| Neighbor/ARP | IP ke MAC | mapping host lokal pada layer 2 |

Contoh membaca alamat:

```text
192.168.1.10/24
```

Artinya:

- IP host: `192.168.1.10`
- Prefix: `/24`
- Network: `192.168.1.0`
- Broadcast umum: `192.168.1.255`
- Host satu subnet biasanya `192.168.1.1` sampai `192.168.1.254`

Urutan berpikir konektivitas:

1. Interface aktif atau tidak.
2. IP dan prefix benar atau tidak.
3. Route default ada atau tidak.
4. DNS resolver benar atau tidak.
5. Service listen di alamat dan port yang benar atau tidak.
6. Firewall host/network mengizinkan traffic atau tidak.

Command:

```bash
# Melihat atau mengubah konfigurasi network Linux.
ip addr

# Melihat atau mengubah konfigurasi network Linux.
ip link

# Melihat atau mengubah konfigurasi network Linux.
ip route

# Melihat atau mengubah konfigurasi network Linux.
ip neigh

# Melihat socket dan port yang sedang listen atau aktif.
ss -tulpn

# Menguji konektivitas dasar ke host tujuan.
ping 8.8.8.8

# Menguji konektivitas dasar ke host tujuan.
ping example.com

# Melacak jalur network menuju host tujuan.
traceroute example.com

# Melacak jalur network menuju host tujuan.
tracepath example.com

# Menguji DNS dan resolver configuration.
dig example.com

# Menguji DNS dan resolver configuration.
nslookup example.com

# Menguji DNS dan resolver configuration.
resolvectl status

# Melihat atau mengubah hostname dan metadata sistem.
hostnamectl
```

File:

```text
/etc/hosts
/etc/resolv.conf
/etc/hostname
/etc/nsswitch.conf
```

`/etc/hosts`:

```text
127.0.0.1   localhost
192.168.1.10 app.local app
```

Field `/etc/hosts`:

| Field | Contoh | Arti |
|---|---|---|
| IP address | `192.168.1.10` | alamat tujuan |
| canonical name | `app.local` | nama utama host |
| alias | `app` | nama tambahan |

`/etc/resolv.conf`:

```text
nameserver 1.1.1.1
nameserver 8.8.8.8
search localdomain
```

Directive `/etc/resolv.conf`:

| Directive | Contoh | Arti |
|---|---|---|
| `nameserver` | `1.1.1.1` | DNS resolver yang dipakai |
| `search` | `localdomain` | domain suffix untuk nama pendek |
| `options` | `timeout:2 attempts:3` | opsi resolver |

`/etc/hostname`:

```text
server01
```

Isi file ini biasanya hanya hostname pendek mesin.

`/etc/nsswitch.conf`:

```text
hosts: files dns
passwd: files sss
group: files sss
```

Field penting `/etc/nsswitch.conf`:

| Database | Contoh sumber | Arti |
|---|---|---|
| `hosts` | `files dns` | cari hostname dari `/etc/hosts`, lalu DNS |
| `passwd` | `files sss` | cari user lokal, lalu SSSD |
| `group` | `files sss` | cari group lokal, lalu SSSD |

NetworkManager:

```bash
# Mengelola koneksi network melalui NetworkManager.
nmcli device status

# Mengelola koneksi network melalui NetworkManager.
nmcli connection show

# Mengelola koneksi network melalui NetworkManager.
nmcli connection show <name>

# Mengelola koneksi network melalui NetworkManager.
nmcli connection modify <name> ipv4.addresses 192.168.1.10/24

# Mengelola koneksi network melalui NetworkManager.
nmcli connection modify <name> ipv4.gateway 192.168.1.1

# Mengelola koneksi network melalui NetworkManager.
nmcli connection modify <name> ipv4.dns 1.1.1.1

# Mengelola koneksi network melalui NetworkManager.
nmcli connection up <name>
```

Interface issues:

- interface down
- wrong IP/prefix
- missing default route
- DNS salah
- firewall block
- service bind ke `127.0.0.1` saja
- MTU mismatch
- bonding salah
- MAC spoofing

Penjelasan:

- Interface `DOWN` berarti link belum aktif secara administratif atau fisik.
- Prefix salah bisa membuat host mengira tujuan remote berada di local network, atau sebaliknya.
- Default route hilang membuat host hanya bisa bicara dengan subnet lokal.
- DNS salah sering terlihat seperti "internet mati", padahal ping ke IP publik masih berhasil.
- MTU mismatch biasanya muncul pada VPN, tunnel, cloud network, atau jumbo frame, dengan gejala koneksi hang pada payload besar.
- Bonding salah bisa menyebabkan packet loss, failover gagal, atau traffic hanya lewat satu link.
- MAC spoofing bisa sengaja dipakai di virtualization, tetapi juga bisa diblokir oleh switch, hypervisor, atau cloud security policy.

### 1.5 Shell Operations

Shell adalah interface utama untuk menjalankan command, menggabungkan tool kecil, membaca file, dan mengotomasi pekerjaan.

Hal yang perlu dipahami sebelum fokus ke command:

- shell membaca input, melakukan expansion, lalu menjalankan command
- path bisa absolute atau relative
- banyak command bekerja dengan file teks dan stream
- pipe menghubungkan output command ke command lain
- exit code menentukan apakah command dianggap berhasil atau gagal
- environment variable memengaruhi perilaku command dan script

Absolute path vs relative path:

| Jenis | Contoh | Arti |
|---|---|---|
| absolute path | `/etc/ssh/sshd_config` | dimulai dari root `/` |
| relative path | `../logs/app.log` | dihitung dari current working directory |

Path penting:

- `.` berarti directory saat ini
- `..` berarti parent directory
- `~` berarti home directory user saat ini
- `-` pada `cd -` berarti directory sebelumnya

Expansion yang dilakukan shell:

| Expansion | Contoh | Arti |
|---|---|---|
| variable | `$HOME` | diganti nilai variable |
| command substitution | `$(date)` | diganti output command |
| globbing | `*.conf` | dicocokkan ke nama file |
| brace expansion | `{a,b}` | menghasilkan beberapa string |
| tilde expansion | `~/notes` | diganti home directory |

#### Navigasi dan Manipulasi File

Navigasi filesystem adalah kemampuan dasar untuk tahu "sedang berada di mana", "file apa yang ada", dan "perubahan apa yang akan dilakukan".

Risiko utama di bagian ini adalah salah path. Karena itu biasakan cek posisi dengan `pwd`, cek isi directory dengan `ls`, dan gunakan command read-only sebelum operasi destructive.

Command dasar:

```bash
# Menampilkan direktori kerja saat ini.
pwd

# Berpindah ke direktori lain.
cd /etc

# Menampilkan isi direktori atau file yang cocok.
ls -lah

# Membuat file kosong atau memperbarui timestamp file.
touch file.txt

# Membuat direktori baru.
mkdir -p dir/subdir

# Menyalin file atau direktori.
cp file copy

# Memindahkan atau mengganti nama file/direktori.
mv old new

# Menghapus file atau direktori, gunakan hati-hati.
rm file

# Menghapus file atau direktori, gunakan hati-hati.
rmdir emptydir

# Mencari file berdasarkan nama, ukuran, tipe, atau kondisi lain.
find /etc -name "*.conf"

# Mencari file dari database locate.
locate sshd_config
```

Penjelasan:

- Command file dasar adalah fondasi semua administrasi Linux karena hampir semua konfigurasi berada dalam file teks.
- `find` membaca filesystem langsung sehingga akurat tetapi bisa lambat pada tree besar.
- `locate` cepat karena memakai database, tetapi hasilnya bisa tidak terbaru jika database belum di-update.
- Saat menghapus atau memindahkan file penting, cek path dengan `pwd`, `ls -l`, dan tab completion agar tidak salah target.
- Untuk operasi yang berulang atau berisiko, mulai dengan command read-only seperti `find` sebelum menambahkan aksi seperti `-delete`.

Melihat isi file:

```bash
# Membaca isi file langsung ke terminal.
cat file

# Membaca sebagian isi file dengan praktis.
less file

# Membaca sebagian isi file dengan praktis.
head file

# Membaca sebagian isi file dengan praktis.
tail file

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/syslog
```

#### Redirection, Pipe, Text Processing

Standard streams:

- stdin: `0`
- stdout: `1`
- stderr: `2`

Penjelasan:

- Redirection mengontrol ke mana output atau error dikirim.
- Pipe mengirim output command pertama menjadi input command berikutnya.
- `stdout` biasanya berisi hasil normal, sedangkan `stderr` berisi error atau diagnostic message.
- Memisahkan stdout dan stderr berguna saat membuat log script.
- Tool seperti `grep`, `awk`, `sed`, `cut`, `sort`, dan `uniq` membuat shell sangat kuat untuk analisis log dan file konfigurasi.
- Untuk perubahan massal pada file, backup dulu dan uji pattern pada sample kecil.

Contoh:

```bash
# Menunjukkan pola redirection atau pipeline pada command umum.
command > out.txt

# Menunjukkan pola redirection atau pipeline pada command umum.
command >> out.txt

# Menunjukkan pola redirection atau pipeline pada command umum.
command 2> err.txt

# Menunjukkan pola redirection atau pipeline pada command umum.
command > all.txt 2>&1

# Menunjukkan pola redirection atau pipeline pada command umum.
command | tee output.txt
```

Text tools:

```bash
# Mencari teks yang cocok dengan pattern.
grep -R "pattern" /etc

# Memproses kolom atau record teks.
awk -F: '{print $1}' /etc/passwd

# Mengedit stream teks secara non-interaktif.
sed 's/old/new/g' file

# Mengambil kolom tertentu dari teks.
cut -d: -f1 /etc/passwd

# Mengurutkan atau menghapus duplikasi baris.
sort file

# Mengurutkan atau menghapus duplikasi baris.
uniq file

# Mentransformasikan karakter pada stream teks.
tr 'a-z' 'A-Z'

# Menghitung baris, kata, atau byte.
wc -l file
```

Globbing:

```bash
# Menampilkan isi direktori atau file yang cocok.
ls *.conf

# Menampilkan isi direktori atau file yang cocok.
ls file?.txt

# Menampilkan isi direktori atau file yang cocok.
ls [abc]*
```

Environment:

```bash
# Mencetak nilai atau teks ke terminal.
echo $PATH

# Membuat variable tersedia untuk child process.
export EDITOR=vim

# Menampilkan environment variable.
env

# Menampilkan environment variable.
printenv

# Mengetahui lokasi atau jenis command.
which ls

# Mengetahui lokasi atau jenis command.
type cd

# Membuat shortcut command di shell.
alias ll='ls -lah'
```

Exit code:

```bash
# Mencetak nilai atau teks ke terminal.
echo $?

# Menjalankan aksi berikutnya hanya jika command pertama berhasil.
cmd && echo success

# Menjalankan aksi fallback jika command pertama gagal.
cmd || echo failed
```

Editor:

```bash
# Membuka file dengan text editor terminal.
nano file

# Membuka file dengan text editor terminal.
vim file
```

Vim basic:

- `i`: insert
- `Esc`: normal mode
- `:w`: save
- `:q`: quit
- `:wq`: save and quit
- `:q!`: quit without save

### 1.6 Virtualization

Konsep:

- hypervisor type 1: bare metal
- hypervisor type 2: hosted
- VM: guest OS dengan virtual hardware
- container: isolasi process, berbagi kernel host

Linux virtualization tools:

- `KVM`: kernel-based virtualization
- `QEMU`: emulator/virtualizer
- `libvirt`: management layer
- `virsh`: CLI libvirt
- `virt-install`: membuat VM
- `qcow2`: format disk image yang mendukung snapshot/sparse

Penjelasan:

- Virtual machine menjalankan kernel sendiri, sehingga isolasinya lebih kuat daripada container tetapi overhead lebih besar.
- KVM adalah fitur kernel Linux untuk hardware-assisted virtualization.
- QEMU menyediakan emulasi dan perangkat virtual; bersama KVM performanya jauh lebih baik.
- libvirt memberi API dan tooling yang lebih nyaman untuk mengelola VM.
- `qcow2` hemat ruang karena sparse dan mendukung snapshot, tetapi raw image kadang lebih cepat untuk workload tertentu.
- Saat VM lambat, cek CPU virtualization support, storage backend, memory pressure, dan driver virtio.

Command:

```bash
# Menampilkan kernel module yang sedang dimuat.
lsmod | grep kvm

# Mengelola virtual machine melalui libvirt.
virsh list --all

# Mengelola virtual machine melalui libvirt.
virsh start vm1

# Mengelola virtual machine melalui libvirt.
virsh shutdown vm1

# Mengelola virtual machine melalui libvirt.
virsh console vm1

# Membuat atau mengubah disk image virtual machine.
qemu-img info disk.qcow2

# Membuat atau mengubah disk image virtual machine.
qemu-img create -f qcow2 disk.qcow2 20G

# Membuat atau mengubah disk image virtual machine.
qemu-img resize disk.qcow2 +10G
```

File/lokasi:

```text
/var/lib/libvirt/images
/etc/libvirt
```

---

## 2.0 Services and User Management

**Praktik setelah bab ini:** hubungkan identity, permission, service, dan log.

```bash
# Melihat identity user saat ini.
id

# Melihat entry user lokal.
getent passwd "$USER"

# Melihat status service SSH jika tersedia.
systemctl status ssh

# Membaca log service SSH dari journal.
journalctl -u ssh --no-pager -n 50
```

Catat: UID/GID, supplementary group, service state, unit file, log error, dan privilege yang dibutuhkan untuk tindakan admin.

Bagian ini merangkum file/permission, user/group, process/job, software, systemd, logs, timers, dan container.

### 2.1 Files, Directories, Links, Permissions

Bagian ini membahas cara Linux melihat object di filesystem dan cara akses ke object tersebut dikontrol.

Ada empat hal yang perlu dipisahkan:

| Konsep | Arti |
|---|---|
| File type | Jenis object: file biasa, directory, symlink, device, socket, dan lain-lain |
| Link | Cara nama file menunjuk ke data atau path lain |
| Ownership | Siapa owner user dan owner group dari object |
| Permission | Aksi apa yang boleh dilakukan owner, group, dan others |

Gambaran mentalnya:

- Nama file berada di directory.
- Nama file menunjuk ke inode.
- Inode menyimpan metadata seperti permission, owner, timestamp, dan lokasi data.
- Permission menentukan apakah user bisa membaca, menulis, atau mengeksekusi.
- Link menentukan apakah beberapa nama menunjuk ke data yang sama atau hanya menunjuk ke path lain.

Kenapa ini penting:

- Banyak error Linux sebenarnya adalah permission error.
- Service bisa gagal start karena tidak bisa membaca config, menulis log, atau mengakses directory.
- SSH key bisa ditolak jika permission terlalu longgar.
- Web server bisa forbidden karena ownership atau SELinux context salah.
- Symlink bisa membingungkan saat target tidak ada, target salah, atau menunjuk ke filesystem lain.

Jenis file:

```bash
# Menampilkan isi direktori atau file yang cocok.
ls -l

# Menampilkan inode number agar hard link bisa dikenali.
ls -li

# Memeriksa tipe file dan metadata filesystem.
file path

# Memeriksa tipe file dan metadata filesystem.
stat path
```

Tipe file di Linux biasanya terlihat dari karakter pertama pada output `ls -l`.

Contoh bentuk output:

```text
-rw-r--r--  1 user user  120 May 23  notes.txt
drwxr-xr-x  2 user user 4096 May 23  Documents
lrwxrwxrwx  1 user user   11 May 23  log -> /var/log
crw-rw----  1 root dialout 4, 64 May 23  /dev/ttyS0
brw-rw----  1 root disk   8,  0 May 23  /dev/sda
srw-rw-rw-  1 user user    0 May 23  app.sock
prw-r--r--  1 user user    0 May 23  mypipe
```

Tabel tipe:

| Tipe | Simbol `ls -l` | Simbol `find -type` | Arti |
|---|---:|---:|---|
| Regular file | `-` | `f` | File biasa seperti text, config, script, binary, image |
| Directory atau folder | `d` | `d` | Wadah yang menyimpan mapping nama file ke inode |
| Symbolic link | `l` | `l` | File khusus yang menunjuk ke path lain |
| Character device | `c` | `c` | Device yang dibaca/ditulis sebagai stream karakter |
| Block device | `b` | `b` | Device storage yang dibaca/ditulis per block |
| Socket | `s` | `s` | Endpoint komunikasi antar process |
| Named pipe atau FIFO | `p` | `p` | Kanal komunikasi satu arah antar process |

Catatan penting tentang hard link:

- Hard link tidak punya simbol tipe file khusus di `ls -l`.
- Hard link adalah nama tambahan yang menunjuk ke inode yang sama.
- Jika hard link dibuat ke regular file, simbolnya tetap `-`.
- Cara mengenali hard link adalah lewat inode number dan link count.

Contoh hard link:

```text
12345 -rw-r--r-- 2 user user 120 May 23 original.txt
12345 -rw-r--r-- 2 user user 120 May 23 hardlink.txt
```

Interpretasi:

- `12345` sama berarti dua nama itu menunjuk inode yang sama.
- Angka `2` setelah permission adalah link count.
- Data baru benar-benar hilang saat semua hard link ke inode tersebut dihapus.

Mencari file berdasarkan tipe:

```bash
# Mencari regular file di directory saat ini.
find . -type f

# Mencari directory atau folder di directory saat ini.
find . -type d

# Mencari symbolic link di directory saat ini.
find . -type l

# Mencari socket di directory saat ini.
find . -type s

# Mencari named pipe atau FIFO di directory saat ini.
find . -type p

# Mencari character device di /dev.
find /dev -type c

# Mencari block device di /dev.
find /dev -type b
```

Penjelasan:

- Regular file berisi data biasa seperti text, binary, config, atau log.
- Directory, atau folder dalam istilah desktop, adalah struktur yang memetakan nama file ke inode.
- Symbolic link menyimpan path menuju target dan bisa menunjuk ke filesystem lain.
- Hard link menunjuk inode yang sama, sehingga data tetap ada selama masih ada hard link.
- Character device dan block device adalah representasi hardware atau pseudo-device di `/dev`.
- Socket dipakai process untuk komunikasi, misalnya Unix socket database atau service lokal.
- Named pipe adalah FIFO untuk komunikasi antar process lewat filesystem.

Link:

```bash
# Membuat atau membaca hard link dan symbolic link.
ln source hardlink

# Membuat atau membaca hard link dan symbolic link.
ln -s target symlink

# Membuat atau membaca hard link dan symbolic link.
readlink symlink
```

#### Inode dan Metadata

Inode adalah struktur data filesystem yang menyimpan metadata file. Nama file biasanya tidak disimpan di inode; nama file disimpan di directory sebagai mapping dari nama ke inode.

Hal yang biasanya ada di inode:

| Metadata | Arti |
|---|---|
| file type | jenis object, misalnya regular file, directory, symlink |
| permission bits | hak akses owner, group, others |
| UID owner | user pemilik file |
| GID group | group pemilik file |
| size | ukuran file |
| timestamps | waktu akses, modifikasi isi, dan perubahan metadata |
| link count | jumlah hard link yang menunjuk inode |
| block pointers | lokasi data di storage |

Cara berpikirnya:

- Directory menyimpan nama.
- Inode menyimpan metadata.
- Data file disimpan di block data.
- Hard link berarti dua nama directory menunjuk inode yang sama.
- Symlink punya inode sendiri dan isinya adalah path menuju target.

Command yang berguna:

```bash
# Menampilkan inode number bersama daftar file.
ls -li

# Melihat metadata file secara detail.
stat file
```

Contoh kasus:

- Jika dua file punya inode number sama, itu hard link.
- Jika symlink target dihapus, symlink menjadi broken link.
- Jika link count file masih lebih dari nol, data belum benar-benar hilang dari filesystem.

#### File Descriptor dan Standard Streams

File descriptor adalah angka kecil yang dipakai process untuk merujuk file, socket, pipe, atau device yang sedang dibuka.

Standard file descriptor:

| FD | Nama | Fungsi |
|---:|---|---|
| `0` | stdin | input standar |
| `1` | stdout | output normal |
| `2` | stderr | output error |

Representasi path:

| Path | Arti |
|---|---|
| `/dev/stdin` | input standar |
| `/dev/stdout` | output standar |
| `/dev/stderr` | output error |

Kenapa ini penting:

- Redirection seperti `>` dan `2>` bekerja dengan file descriptor.
- Pipe menghubungkan stdout command pertama ke stdin command berikutnya.
- Log script yang rapi biasanya memisahkan output normal dan output error.
- `lsof` bisa menunjukkan file descriptor apa saja yang dibuka process.

#### Buffered dan Unbuffered I/O

I/O adalah operasi baca/tulis. Buffer adalah area memory sementara untuk menampung data sebelum benar-benar dibaca/ditulis.

Buffered I/O:

- lebih efisien untuk data besar
- data dikumpulkan dulu sebelum ditulis
- output bisa terlihat terlambat karena masih berada di buffer
- umum dipakai aplikasi dan library standar

Unbuffered I/O:

- lebih langsung ke kernel atau device
- berguna untuk komunikasi yang perlu respons segera
- bisa lebih lambat jika dilakukan byte-per-byte untuk data besar

Kenapa ini penting saat administrasi:

- Log aplikasi kadang tidak langsung muncul karena buffering.
- Script pipeline bisa terlihat "diam" padahal data masih ditahan buffer.
- Operasi storage besar bisa tampak selesai di aplikasi, tetapi data masih perlu flush ke disk.
- Command seperti `sync` atau mekanisme fsync aplikasi penting untuk memastikan data benar-benar tertulis.

#### Device, Pseudo Device, dan Virtual Filesystem

Linux sering memakai file sebagai interface ke hal yang bukan file biasa. Ini bagian dari ide "everything is a file".

Contoh:

| Path | Jenis | Fungsi |
|---|---|---|
| `/dev/sda` | block device | disk SATA/SCSI pertama |
| `/dev/nvme0n1` | block device | disk NVMe |
| `/dev/tty` | character device | terminal saat ini |
| `/dev/null` | pseudo device | membuang semua input |
| `/dev/zero` | pseudo device | menghasilkan byte nol |
| `/dev/random` | pseudo device | random data dari entropy pool |
| `/dev/urandom` | pseudo device | random data non-blocking |
| `/dev/loop*` | loop device | file yang diperlakukan seperti block device |
| `/proc` | virtual filesystem | informasi runtime process dan kernel |
| `/sys` | virtual filesystem | informasi device dan kernel object |

Bedanya:

- Device file di `/dev` adalah interface ke device atau pseudo-device.
- `/proc` dan `/sys` bukan data biasa di disk; isinya dibuat kernel secara runtime.
- Banyak file di `/proc` dan `/sys` berubah sesuai kondisi sistem saat itu.

Contoh penggunaan umum:

```bash
# Membuang output command ke tempat kosong.
command > /dev/null

# Melihat informasi memory dari virtual filesystem /proc.
cat /proc/meminfo

# Melihat informasi process tertentu dari /proc.
cat /proc/<PID>/status

# Melihat block device yang dikenali kernel.
lsblk
```

Ownership:

```bash
# Mengatur ownership, permission, atau default permission.
chown user file

# Mengatur ownership, permission, atau default permission.
chown user:group file

# Mengatur ownership, permission, atau default permission.
chgrp group file
```

Permission:

```bash
# Mengatur ownership, permission, atau default permission.
chmod 644 file

# Mengatur ownership, permission, atau default permission.
chmod 755 dir

# Mengatur ownership, permission, atau default permission.
chmod u+x script.sh

# Mengatur ownership, permission, atau default permission.
umask
```

Special bits:

```bash
# Mengatur ownership, permission, atau default permission.
chmod u+s file

# Mengatur ownership, permission, atau default permission.
chmod g+s dir

# Mengatur ownership, permission, atau default permission.
chmod +t /shared
```

Makna:

- SUID: executable berjalan dengan owner file
- SGID: executable berjalan dengan group file, atau file baru di directory mengikuti group directory
- sticky bit: hanya owner file/root yang bisa hapus file di directory

Penjelasan:

- Permission dasar Linux terdiri dari read, write, execute untuk owner, group, dan others.
- Pada file, execute berarti file bisa dijalankan sebagai program/script.
- Pada directory, execute berarti bisa masuk/traverse directory tersebut.
- `umask` mengurangi permission default saat file atau directory baru dibuat.
- SUID dan SGID pada executable harus diaudit karena bisa memberi privilege lebih tinggi.
- Sticky bit umum di `/tmp`, agar user bisa membuat file tetapi tidak bebas menghapus file user lain.
- ACL dipakai ketika model owner/group/others tidak cukup granular.

ACL:

```bash
# Melihat atau mengubah Access Control List file.
getfacl file

# Melihat atau mengubah Access Control List file.
setfacl -m u:alice:rwx file

# Melihat atau mengubah Access Control List file.
setfacl -x u:alice file
```

### 2.2 Account Management

Bagian ini membahas identitas lokal di Linux: user, group, password, shell, home directory, dan privilege.

Konsep utama:

| Konsep | Arti |
|---|---|
| User | Identitas yang dipakai untuk login atau menjalankan process |
| Group | Kumpulan user untuk mempermudah pemberian akses |
| Password database | Informasi akun dan autentikasi lokal |
| Home directory | Area kerja default user |
| Login shell | Program shell yang dijalankan saat user login |
| Sudo rule | Aturan privilege escalation untuk menjalankan command tertentu sebagai user lain |

Gambaran mentalnya:

- User bukan hanya orang. Service seperti `nginx`, `mysql`, atau `sshd` juga bisa berjalan dengan user sendiri.
- Group mempermudah akses bersama, misalnya group `developers` boleh menulis ke directory project.
- Password hash tidak disimpan di `/etc/passwd`, tetapi di `/etc/shadow`.
- Saat user menjalankan process, process itu membawa UID, GID, supplementary groups, environment, dan permission user tersebut.
- `sudo` bukan sekadar "menjadi root"; `sudo` adalah policy tentang siapa boleh menjalankan command apa sebagai siapa.

Kenapa ini penting:

- Login gagal bisa disebabkan password, account lock, shell invalid, expired account, home permission, atau PAM.
- Akses file sering ditentukan oleh kombinasi user, primary group, supplementary group, ACL, dan permission directory parent.
- Service account yang terlalu privileged memperbesar dampak jika service dieksploitasi.
- Menghapus user tanpa memahami ownership file bisa meninggalkan file dengan UID tanpa nama user.

File penting:

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
/etc/login.defs
/etc/skel
/etc/profile
```

#### File Identitas Lokal

File identity lokal adalah sumber dasar untuk user dan group pada mesin standalone. Pada environment enterprise, data bisa juga datang dari LDAP, Active Directory, atau SSSD, tetapi file lokal tetap penting untuk recovery dan akun sistem.

`/etc/passwd`:

```text
alice:x:1001:1001:Alice:/home/alice:/bin/bash
```

Field `/etc/passwd`:

| Field | Contoh | Arti |
|---|---|---|
| username | `alice` | nama login |
| password placeholder | `x` | password hash disimpan di `/etc/shadow` |
| UID | `1001` | user ID |
| GID | `1001` | primary group ID |
| GECOS | `Alice` | informasi deskriptif user |
| home | `/home/alice` | home directory |
| shell | `/bin/bash` | login shell |

`/etc/shadow`:

```text
alice:$y$j9T$...:19800:0:99999:7:::
```

Field penting `/etc/shadow`:

| Field | Arti |
|---|---|
| username | user yang memiliki password entry |
| password hash | hash password atau tanda lock seperti `!` |
| last change | hari terakhir password diganti, dihitung dari epoch |
| min age | minimal hari sebelum boleh ganti password |
| max age | maksimal umur password |
| warning | jumlah hari warning sebelum expired |
| inactive | masa grace setelah expired |
| expire | tanggal account expired |

`/etc/group`:

```text
developers:x:1002:alice,bob
```

Field `/etc/group`:

| Field | Contoh | Arti |
|---|---|---|
| group name | `developers` | nama group |
| password placeholder | `x` | jarang dipakai langsung |
| GID | `1002` | group ID |
| members | `alice,bob` | supplementary members |

Jenis user:

| Jenis | Arti | Catatan |
|---|---|---|
| root | superuser UID `0` | punya kontrol penuh, harus dipakai hati-hati |
| regular user | user manusia biasa | dipakai untuk login harian |
| service user | user untuk daemon/service | sebaiknya minimal privilege dan non-login |

Shell non-login yang sering dipakai service account:

```text
/usr/sbin/nologin
/sbin/nologin
/bin/false
```

User/group commands:

```bash
# Mengelola akun user, group, shell, password, atau masa berlaku akun.
useradd alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
adduser alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
passwd alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
usermod -aG sudo alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
usermod -s /bin/bash alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
usermod -L alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
usermod -U alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
userdel alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
userdel -r alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
groupadd dev

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
groupmod -n developers dev

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
groupdel dev

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
chsh -s /bin/bash alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
chage -l alice

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
chage -M 90 alice
```

Query:

```bash
# Melihat identitas, group, login, atau database akun.
id alice

# Melihat identitas, group, login, atau database akun.
groups alice

# Melihat identitas, group, login, atau database akun.
getent passwd alice

# Melihat identitas, group, login, atau database akun.
getent group sudo

# Melihat identitas, group, login, atau database akun.
whoami

# Melihat identitas, group, login, atau database akun.
who

# Melihat identitas, group, login, atau database akun.
w

# Melihat identitas, group, login, atau database akun.
last

# Melihat identitas, group, login, atau database akun.
lastlog
```

UID/GID:

- UID: user ID
- GID: primary group ID
- EUID: effective user ID
- EGID: effective group ID
- system account: biasanya UID rendah atau range khusus distro
- service account: akun untuk menjalankan service

Penjelasan:

- Kernel lebih peduli UID/GID daripada nama user/group. Nama hanya mapping yang dibaca dari database identity.
- Primary group adalah group default untuk file baru, kecuali directory memakai SGID atau ACL default.
- Supplementary group memberi akses tambahan seperti `sudo`, `docker`, atau group aplikasi.
- EUID/EGID menentukan privilege efektif saat process berjalan, penting untuk memahami SUID/SGID.
- Service account sebaiknya punya privilege minimum dan shell non-login jika tidak perlu login interaktif.
- Jangan berbagi satu akun admin untuk banyak orang. Gunakan akun personal dan audit lewat `sudo`.

Sudo:

```bash
# Melihat hak sudo untuk user saat ini.
sudo -l

# Mengelola atau memakai privilege administratif secara terkontrol.
visudo

# Mengelola atau memakai privilege administratif secara terkontrol.
sudoedit /etc/sudoers.d/admins
```

Contoh sudoers:

```text
%wheel ALL=(ALL) ALL
alice ALL=(root) NOPASSWD: /usr/bin/systemctl restart nginx
```

Format dasar sudoers:

```text
user_or_group host=(run_as_user:run_as_group) options: command
```

Field sudoers:

| Field | Contoh | Arti |
|---|---|---|
| user/group | `alice`, `%wheel` | user atau group yang diberi hak |
| host | `ALL` | host tempat rule berlaku |
| run as | `(root)` atau `(ALL:ALL)` | user/group target saat command dijalankan |
| options | `NOPASSWD:` | opsi tambahan seperti tanpa password |
| command | `/usr/bin/systemctl restart nginx` | command yang boleh dijalankan |

Contoh interpretasi:

```text
alice ALL=(root) NOPASSWD: /usr/bin/systemctl restart nginx
```

Artinya:

- user `alice` boleh menjalankan command tertentu
- rule berlaku di semua host
- command dijalankan sebagai `root`
- tidak perlu memasukkan password saat menjalankan command itu
- haknya hanya untuk restart `nginx`, bukan semua command

`su` vs `sudo`:

| Tool | Fungsi | Catatan |
|---|---|---|
| `su` | berpindah user atau membuka shell sebagai user lain | biasanya butuh password target user |
| `su -` | login shell sebagai user lain | memuat environment login user target |
| `sudo` | menjalankan command sesuai policy sudoers | biasanya memakai password user sendiri |
| `sudo -i` | membuka login shell root | lebih mudah diaudit daripada login root langsung |
| `sudoedit` | mengedit file privileged secara lebih aman | editor berjalan dengan environment user |

Prinsip aman:

- gunakan `sudo` untuk aksi admin yang spesifik
- hindari login root langsung jika tidak perlu
- edit sudoers lewat `visudo` agar syntax divalidasi
- taruh aturan tambahan di `/etc/sudoers.d/` agar mudah dikelola
- jangan memberi `NOPASSWD: ALL` kecuali benar-benar paham risikonya

### 2.3 Processes, Jobs, and Scheduling

Bagian ini sebenarnya terdiri dari tiga konsep yang saling berhubungan, tetapi tidak sama:

| Konsep | Arti Sederhana | Contoh |
|---|---|---|
| Process | Program yang sedang berjalan di sistem | `nginx`, `sshd`, `bash`, `python3 script.py` |
| Job | Process atau pipeline yang dikelola oleh shell saat ini | `sleep 100 &`, `grep error log | less` |
| Scheduling | Menjalankan command secara otomatis pada waktu tertentu | cronjob backup tiap malam |

Gambaran mentalnya:

- Ketika kamu menjalankan command, shell membuat process.
- Jika command itu berjalan di terminal yang sama, ia menjadi foreground job.
- Jika command diberi `&`, ia menjadi background job.
- Jika command dijadwalkan lewat `cron`, `at`, atau `systemd timer`, ia akan dibuat menjadi process pada waktu tertentu tanpa kamu mengetik manual.

#### Process

Process adalah instance program yang sedang berjalan. Satu program bisa punya banyak process. Misalnya, web server bisa punya master process dan beberapa worker process.

Hal penting dalam process:

- PID adalah nomor unik process.
- PPID adalah PID parent process.
- User menentukan permission process.
- Environment variable memengaruhi perilaku process.
- File descriptor menunjukkan file, socket, atau pipe yang sedang dibuka process.
- Current working directory memengaruhi path relatif.
- Signal dipakai untuk meminta process berhenti, reload, atau melanjutkan eksekusi.

Lifecycle sederhana process:

1. Process dibuat, biasanya lewat `fork()` lalu `exec()`.
2. Process berjalan dengan user, environment, dan resource tertentu.
3. Process bisa membuat child process.
4. Process selesai dengan exit code.
5. Parent process mengambil exit status child.

Process state yang sering muncul:

| State | Arti |
|---|---|
| `R` | Running atau siap berjalan di CPU |
| `S` | Sleeping, menunggu event biasa |
| `D` | Uninterruptible sleep, sering menunggu I/O |
| `T` | Stopped atau suspended |
| `Z` | Zombie, sudah selesai tetapi parent belum mengambil status |

Kenapa ini penting:

- CPU tinggi berarti ada process memakai CPU besar.
- Memory tinggi berarti ada process memakai RAM besar atau memory leak.
- Banyak process state `D` sering mengarah ke masalah disk, NFS, atau storage.
- Zombie banyak biasanya menandakan parent process bermasalah.
- Process bisa gagal karena permission, environment, path, dependency, atau limit resource.

Process inspection:

```bash
# Melihat process dan penggunaan resource.
ps aux

# Melihat process dan penggunaan resource.
ps -ef

# Melihat process dan penggunaan resource.
pstree -p

# Melihat process dan penggunaan resource.
top

# Melihat process dan penggunaan resource.
htop

# Melihat process dan penggunaan resource.
atop

# Melihat file atau socket yang sedang dibuka process.
lsof

# Melacak system call untuk debugging process.
strace -p <PID>

# Membaca isi file langsung ke terminal.
cat /proc/<PID>/status
```

Performance/process tools:

```bash
# Membaca metrik performa CPU, memory, load, atau I/O.
mpstat

# Membaca metrik performa CPU, memory, load, atau I/O.
pidstat

# Membaca metrik performa CPU, memory, load, atau I/O.
vmstat

# Membaca metrik performa CPU, memory, load, atau I/O.
iostat

# Membaca metrik performa CPU, memory, load, atau I/O.
sar

# Membaca metrik performa CPU, memory, load, atau I/O.
free -h

# Membaca metrik performa CPU, memory, load, atau I/O.
uptime
```

PID concepts:

- PID: process ID
- PPID: parent process ID
- zombie: process selesai tapi belum di-reap parent
- orphan: parent mati, biasanya diadopsi init/systemd

Penjelasan:

- Process adalah program yang sedang berjalan dengan PID, memory, environment, file descriptor, dan permission sendiri.
- Parent-child relationship penting untuk memahami service supervisor, shell, dan zombie process.
- Zombie tidak memakai CPU besar, tetapi menandakan parent tidak mengambil exit status child.
- Orphan process biasanya diadopsi `systemd` agar tetap punya parent.
- Saat process memakan resource tinggi, cek bukan hanya process tunggal, tetapi juga parent, command line, open files, dan log service.

Priority:

```bash
# Mengatur prioritas scheduling process.
nice -n 10 command

# Mengatur prioritas scheduling process.
renice -n 5 -p <PID>
```

Penjelasan priority:

- Linux scheduler menentukan process mana yang mendapat jatah CPU.
- Nice value memengaruhi prioritas CPU untuk process normal.
- Nilai nice lebih kecil berarti prioritas lebih tinggi, sedangkan nilai lebih besar berarti process lebih "mengalah".
- User biasa biasanya hanya bisa menaikkan nilai nice, bukan menurunkannya ke prioritas lebih tinggi.
- `nice` dipakai saat menjalankan process baru, sedangkan `renice` dipakai untuk process yang sudah berjalan.
- Nice tidak menyelesaikan bottleneck I/O, memory, atau network. Ia hanya memengaruhi scheduling CPU.

Signals:

```bash
# Mengirim signal untuk menghentikan atau mengontrol process.
kill -15 <PID>

# Mengirim signal untuk menghentikan atau mengontrol process.
kill -9 <PID>

# Mengirim signal untuk menghentikan atau mengontrol process.
kill -HUP <PID>

# Mengirim signal untuk menghentikan atau mengontrol process.
killall nginx

# Mengirim signal untuk menghentikan atau mengontrol process.
pkill -f pattern
```

Umum:

- `1` HUP: reload/hangup
- `9` KILL: paksa mati, tidak bisa ditangkap process
- `15` TERM: request terminate normal

Penjelasan signal:

- Signal adalah cara kernel atau user mengirim notifikasi ke process.
- `SIGTERM` adalah cara sopan meminta process berhenti, sehingga process masih bisa cleanup.
- `SIGKILL` memaksa process mati tanpa cleanup, jadi pakai sebagai pilihan terakhir.
- `SIGHUP` sering dipakai service lama untuk reload konfigurasi.
- Jika process tidak mati dengan `SIGTERM`, cek apakah process sedang di state `D`, stuck I/O, atau dikelola supervisor yang langsung menyalakannya lagi.

#### Job

Job adalah process atau kumpulan process yang sedang dikelola oleh shell interaktif. Job bukan konsep global seluruh sistem; job biasanya hanya berlaku di session shell tempat command dijalankan.

Contoh:

- `sleep 100` berjalan sebagai foreground job.
- `sleep 100 &` berjalan sebagai background job.
- `grep error app.log | less` adalah satu job yang terdiri dari pipeline beberapa process.

Hal penting dalam job control:

- Foreground job menerima input langsung dari terminal.
- Background job tetap berjalan tetapi tidak menguasai terminal.
- `Ctrl+c` mengirim interrupt ke foreground job.
- `Ctrl+z` men-suspend foreground job.
- `fg` membawa job ke foreground.
- `bg` melanjutkan job di background.
- `nohup`, `disown`, `tmux`, atau `screen` dipakai agar pekerjaan tetap berjalan setelah terminal ditutup.

Kapan ini dipakai:

- menjalankan command lama sambil tetap memakai terminal
- menghentikan sementara command yang sedang berjalan
- menjaga proses manual tetap hidup saat koneksi SSH rawan putus
- mengelola pipeline saat eksplorasi log atau data besar

Job control:

```bash
# Menunjukkan pola redirection atau pipeline pada command umum.
command &

# Mengelola job foreground/background di shell.
jobs

# Mengelola job foreground/background di shell.
fg %1

# Mengelola job foreground/background di shell.
bg %1

# Mengirim interrupt ke process foreground.
Ctrl+c

# Men-suspend process foreground agar bisa dilanjutkan sebagai job.
Ctrl+z

# Mengelola job foreground/background di shell.
nohup command &

# Mengelola job foreground/background di shell.
disown

# Mengelola job foreground/background di shell.
exec command
```

#### Scheduling

Scheduling berarti membuat sistem menjalankan command tanpa interaksi manual pada waktu atau kondisi tertentu.

Contoh kebutuhan scheduling:

- backup database setiap malam
- rotasi atau cleanup file temporary
- sinkronisasi file ke server lain
- health check berkala
- menjalankan report setiap awal bulan
- restart service non-kritis di maintenance window

Perbedaan dengan job:

- Job biasanya terkait session shell interaktif saat ini.
- Scheduled task tidak bergantung pada terminal yang sedang dibuka.
- Scheduled task perlu environment yang eksplisit karena tidak selalu membaca profile shell user.
- Scheduled task harus punya logging karena tidak ada user yang melihat output langsung.

Scheduling:

```bash
# Menjadwalkan pekerjaan otomatis.
crontab -e

# Menjadwalkan pekerjaan otomatis.
crontab -l

# Menjadwalkan pekerjaan otomatis.
sudo crontab -u user -l

# Menjadwalkan pekerjaan otomatis.
at now + 5 minutes

# Menjadwalkan pekerjaan otomatis.
atq

# Menjadwalkan pekerjaan otomatis.
atrm <jobid>

# Mengelola atau memeriksa unit dan service systemd.
systemctl list-timers
```

Cron format:

```text
* * * * * command
| | | | |
| | | | +-- day of week
| | | +---- month
| | +------ day of month
| +-------- hour
+---------- minute
```

Arti field cron:

| Field | Range umum | Arti |
|---|---|---|
| minute | `0-59` | menit |
| hour | `0-23` | jam |
| day of month | `1-31` | tanggal |
| month | `1-12` | bulan |
| day of week | `0-7` | hari, biasanya `0` dan `7` sama-sama Sunday |

Contoh jadwal:

| Jadwal | Arti |
|---|---|
| `* * * * *` | setiap menit |
| `0 * * * *` | setiap awal jam |
| `0 2 * * *` | setiap hari jam 02:00 |
| `*/5 * * * *` | setiap 5 menit |
| `0 0 * * 0` | setiap Sunday tengah malam |

Special string:

| String | Arti |
|---|---|
| `@reboot` | saat boot |
| `@hourly` | setiap jam |
| `@daily` | setiap hari |
| `@weekly` | setiap minggu |
| `@monthly` | setiap bulan |
| `@yearly` | setiap tahun |

User crontab vs system-wide cron:

| Lokasi | Format | Catatan |
|---|---|---|
| user crontab | `* * * * * command` | diedit dengan `crontab -e` |
| `/etc/crontab` | `* * * * * user command` | punya field user |
| `/etc/cron.d/` | `* * * * * user command` | cocok untuk package/service |
| `/etc/cron.daily/` | script | dijalankan harian oleh cron/anacron |

Environment cron:

- `PATH` biasanya minimal.
- Working directory belum tentu seperti saat command dijalankan manual.
- Shell default bisa `/bin/sh`, bukan Bash.
- Output stdout/stderr biasanya dikirim via mail lokal jika tidak diredirect.

Overlap:

- Jika job lebih lama dari interval jadwal, dua instance bisa berjalan bersamaan.
- Gunakan lock seperti `flock` untuk mencegah overlap.
- Job yang mengubah file, backup, atau database harus punya proteksi overlap.

Anacron:

- cocok untuk job periodik di mesin yang tidak selalu hidup
- biasanya menjalankan job harian/mingguan/bulanan yang terlewat

Penjelasan:

- Scheduling berarti membuat sistem menjalankan command tanpa interaksi manual pada waktu atau kondisi tertentu.
- `cron` cocok untuk jadwal berulang yang presisi, tetapi environment-nya minimal.
- Script yang berhasil manual bisa gagal di cron karena `PATH`, working directory, permission, atau environment variable berbeda.
- `at` cocok untuk satu kali eksekusi di masa depan.
- `systemd timer` lebih modern untuk banyak service karena bisa terintegrasi dengan dependency, logging, dan unit status.
- Untuk job penting, selalu log output, pakai locking seperti `flock`, dan buat retry/alert jika gagal.

Perbedaan scheduler:

| Scheduler | Cocok Untuk | Catatan |
|---|---|---|
| `cron` | tugas berulang sederhana | environment minimal, format jadwal ringkas |
| `anacron` | mesin yang tidak selalu menyala | menjalankan job periodik yang terlewat |
| `at` | tugas satu kali | cocok untuk eksekusi nanti sekali saja |
| `systemd timer` | tugas yang butuh logging/dependency rapi | terintegrasi dengan unit systemd |

Workflow aman membuat scheduled task:

1. Buat script dan jalankan manual.
2. Pakai path absolut untuk command penting.
3. Tambahkan logging.
4. Tambahkan lock agar tidak overlap.
5. Jalankan lewat scheduler.
6. Cek log hasil eksekusi pertama.

### 2.4 Software Management

Bagian ini membahas cara Linux memasang, menghapus, memperbarui, dan memverifikasi software.

Konsep utama:

| Konsep | Arti |
|---|---|
| Package | Bundle software berisi binary, library, metadata, dan script install/remove |
| Repository | Sumber package yang dipercaya oleh package manager |
| Dependency | Package lain yang dibutuhkan agar software berjalan |
| Package database | Catatan package apa saja yang terpasang dan file apa yang dimiliki |
| Signature | Mekanisme untuk memverifikasi asal dan integritas package |

Gambaran mentalnya:

- `apt` dan `dnf` adalah package manager tingkat tinggi yang mengurus repository dan dependency.
- `dpkg` dan `rpm` adalah tool tingkat rendah yang tahu isi package dan database lokal.
- Update metadata repository tidak sama dengan upgrade package.
- Remove menghapus package, sedangkan purge pada Debian/Ubuntu juga menghapus konfigurasi tertentu.
- Package dari source code manual biasanya tidak tercatat rapi oleh package manager distro.

Kenapa ini penting:

- Dependency rusak bisa membuat install/upgrade gagal.
- Repository pihak ketiga bisa membawa versi package yang konflik dengan repository distro.
- Upgrade tanpa membaca perubahan bisa mengganti config, restart service, atau mengubah behavior aplikasi.
- Verifikasi package membantu mendeteksi file package yang berubah tanpa sepengetahuan package manager.

APT:

```bash
# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt update

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt install nginx

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt upgrade

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt full-upgrade

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt remove nginx

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt purge nginx

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt autoremove

# Mengelola package dan repository pada distro Debian/Ubuntu.
apt search nginx

# Mengelola package dan repository pada distro Debian/Ubuntu.
apt show nginx
```

DPKG:

```bash
# Mengelola package DEB tingkat rendah.
dpkg -l

# Mengelola package DEB tingkat rendah.
dpkg -L package

# Mengelola package DEB tingkat rendah.
sudo dpkg -i package.deb

# Mengelola package DEB tingkat rendah.
sudo dpkg -r package
```

DNF/YUM:

```bash
# Mengelola package dan repository pada distro RPM-based.
sudo dnf install nginx

# Mengelola package dan repository pada distro RPM-based.
sudo dnf update

# Mengelola package dan repository pada distro RPM-based.
sudo dnf remove nginx

# Mengelola package dan repository pada distro RPM-based.
dnf search nginx

# Mengelola package dan repository pada distro RPM-based.
dnf info nginx

# Mengelola package dan repository pada distro RPM-based.
dnf provides */sshd
```

RPM:

```bash
# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -qa

# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -qi package

# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -ql package

# Mengelola atau memverifikasi package RPM tingkat rendah.
sudo rpm -ivh package.rpm

# Mengelola atau memverifikasi package RPM tingkat rendah.
sudo rpm -e package

# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -V package
```

Repositories:

```bash
# Mengelola package dan repository pada distro Debian/Ubuntu.
apt-cache policy

# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo add-apt-repository ppa:name/ppa

# Mengelola package dan repository pada distro RPM-based.
dnf repolist

# Mengelola package dan repository pada distro RPM-based.
sudo dnf config-manager --add-repo <url>
```

Language-specific package managers:

```bash
# Menjalankan Python atau mengelola package Python.
pip install package

# Menginstal package JavaScript/Node.js.
npm install package

# Menginstal package atau binary Rust melalui Cargo.
cargo install package
```

Source install basics:

```bash
# Membuat atau mengekstrak archive.
tar -xvf source.tar.gz

# Menyiapkan konfigurasi build dari source code.
./configure

# Mengompilasi source code sesuai Makefile.
make

# Menginstal hasil build ke sistem.
sudo make install
```

Risiko source install:

- tidak selalu terdaftar di package manager
- sulit update/remove
- dependency conflict

Penjelasan:

- Package manager menjaga daftar file yang terpasang, dependency, versi, signature, dan repository asal package.
- Gunakan package manager distro sebagai pilihan utama karena update security dan dependency lebih mudah dikontrol.
- Third-party repository bisa berguna, tetapi menambah risiko trust, conflict, dan supply chain.
- Package hold/pin dipakai untuk menahan versi tertentu, tetapi harus dicatat agar update security tidak terlewat.
- Source install sebaiknya dipakai ketika tidak ada package resmi atau butuh build option khusus.
- Sebelum upgrade besar, cek changelog, backup konfigurasi, dan pastikan ada rollback path.

Update vs upgrade:

| Istilah | Arti |
|---|---|
| update metadata | mengambil daftar package terbaru dari repository |
| upgrade package | memasang versi package yang lebih baru |
| full upgrade / distro sync | boleh menambah, mengganti, atau menghapus package untuk menyelesaikan dependency |

Pada Debian/Ubuntu:

- `apt update` hanya refresh metadata.
- `apt upgrade` meng-upgrade package tanpa perubahan dependency besar.
- `apt full-upgrade` boleh menghapus/menambah package jika diperlukan.

Package state yang sering ditemui:

| State | Arti |
|---|---|
| installed | package terpasang |
| removed | binary dihapus, konfigurasi tertentu bisa masih ada |
| purged | package dan konfigurasi tertentu dihapus |
| held | package ditahan agar tidak ikut upgrade |
| broken | dependency/configuration belum beres |

Package hold:

- Dipakai untuk menahan versi package tertentu.
- Berguna jika versi baru merusak aplikasi.
- Berbahaya jika membuat security update tertahan terlalu lama.
- Catat alasan hold dan jadwalkan review ulang.

Verifikasi integritas package:

- `rpm -V package` membandingkan file package terpasang dengan database RPM.
- `debsums` bisa memeriksa checksum file package pada Debian-based distro.
- Perubahan pada file konfigurasi bisa normal, tetapi perubahan binary perlu dicurigai.

Universal package:

| Tool | Catatan |
|---|---|
| Snap | package sandboxed, umum di Ubuntu |
| Flatpak | umum untuk aplikasi desktop |
| AppImage | aplikasi portable berbentuk satu file |

Catatan:

- Universal package bisa membawa dependency sendiri.
- Update, permission, dan lokasi file bisa berbeda dari package distro.
- Saat troubleshooting aplikasi, cek apakah aplikasi berasal dari package distro, Snap, Flatpak, AppImage, container, atau source install.

Troubleshooting package:

```bash
# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt -f install

# Mengelola package DEB tingkat rendah.
sudo dpkg --configure -a

# Mengelola package dan repository pada distro RPM-based.
sudo dnf clean all

# Mengelola package dan repository pada distro RPM-based.
sudo dnf check

# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -Va
```

### 2.5 Systems Management with systemd

Bagian ini membahas `systemd`, yaitu init system dan service manager yang banyak dipakai distro modern.

Konsep utama:

| Konsep | Arti |
|---|---|
| Unit | Object yang dikelola systemd, seperti service, timer, socket, mount, target |
| Service unit | Definisi cara menjalankan daemon atau background service |
| Target | Kumpulan unit yang merepresentasikan state sistem |
| Timer | Jadwal untuk menjalankan unit lain |
| Journal | Sistem logging bawaan systemd |
| Dependency | Hubungan urutan dan kebutuhan antar unit |

Gambaran mentalnya:

- Saat boot, systemd membaca target default.
- Target default menarik unit lain lewat dependency.
- Service unit menjalankan daemon seperti `sshd`, `nginx`, atau `postgresql`.
- Jika service gagal, systemd menyimpan status exit, log, dan restart count.
- Timer bisa menjalankan service pada jadwal tertentu, mirip cron tetapi dengan integrasi systemd.

Kenapa ini penting:

- Banyak service Linux modern dikelola lewat `systemctl`.
- Error service biasanya paling cepat dibaca lewat `systemctl status` dan `journalctl -u`.
- `enable` dan `start` berbeda: satu untuk boot berikutnya, satu untuk sekarang.
- Unit bawaan package sebaiknya tidak diedit langsung; gunakan override.
- Dependency yang salah bisa membuat service start terlalu cepat, terlalu lambat, atau gagal saat boot.

Jenis unit yang sering ditemui:

| Suffix | Fungsi |
|---|---|
| `.service` | daemon atau background process |
| `.socket` | socket activation |
| `.timer` | penjadwalan unit |
| `.mount` | mount point |
| `.automount` | mount on-demand |
| `.target` | grouping state sistem |
| `.path` | trigger berdasarkan perubahan path |

Struktur unit service sederhana:

```text
[Unit]
Description=Contoh service
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/app
Restart=on-failure
User=appuser
Group=appuser

[Install]
WantedBy=multi-user.target
```

Bagian penting:

| Section | Fungsi |
|---|---|
| `[Unit]` | metadata dan dependency |
| `[Service]` | cara menjalankan service |
| `[Install]` | cara unit di-enable ke target |

Directive umum:

| Directive | Arti |
|---|---|
| `After=` | urutan start, bukan dependency wajib |
| `Requires=` | dependency wajib, jika gagal unit ikut gagal |
| `Wants=` | dependency lemah |
| `ExecStart=` | command utama service |
| `Restart=` | policy restart otomatis |
| `User=` | user yang menjalankan service |
| `Environment=` | environment variable untuk service |

Target penting:

| Target | Arti |
|---|---|
| `multi-user.target` | mode server multi-user tanpa GUI |
| `graphical.target` | mode dengan GUI |
| `rescue.target` | mode rescue dengan service minimal |
| `emergency.target` | mode sangat minimal untuk recovery |

Service:

```bash
# Mengelola atau memeriksa unit dan service systemd.
systemctl status nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl start nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl stop nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl restart nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl reload nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl enable nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl disable nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl enable --now nginx

# Mengelola atau memeriksa unit dan service systemd.
systemctl is-enabled nginx

# Mengelola atau memeriksa unit dan service systemd.
systemctl is-active nginx
```

Unit inspection:

```bash
# Mengelola atau memeriksa unit dan service systemd.
systemctl list-units

# Mengelola atau memeriksa unit dan service systemd.
systemctl list-unit-files

# Mengelola atau memeriksa unit dan service systemd.
systemctl cat nginx

# Mengelola atau memeriksa unit dan service systemd.
systemctl show nginx

# Mengelola atau memeriksa unit dan service systemd.
systemctl daemon-reload
```

Unit override:

```bash
# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl edit nginx

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl daemon-reload

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl restart nginx
```

Journal:

```bash
# Membaca log dari systemd journal.
journalctl

# Membaca log dari systemd journal.
journalctl -xe

# Membaca log dari systemd journal.
journalctl -u nginx

# Membaca log dari systemd journal.
journalctl -f

# Membaca log dari systemd journal.
journalctl --since "1 hour ago"

# Membaca log dari systemd journal.
journalctl -p err
```

Timer:

```bash
# Mengelola atau memeriksa unit dan service systemd.
systemctl list-timers

# Mengelola atau memeriksa unit dan service systemd.
systemctl status apt-daily.timer
```

Target:

```bash
# Mengelola atau memeriksa unit dan service systemd.
systemctl get-default

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl set-default multi-user.target

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl isolate graphical.target
```

Penjelasan:

- `systemd` mengelola service, mount, timer, socket, target, dan dependency antar unit.
- `start` hanya menjalankan service sekarang, sedangkan `enable` membuat service auto-start saat boot.
- `restart` menghentikan lalu menjalankan ulang service, sedangkan `reload` meminta service membaca ulang konfigurasi tanpa full restart jika didukung.
- `daemon-reload` diperlukan setelah mengubah unit file agar systemd membaca definisi terbaru.
- Override lewat `systemctl edit` lebih aman daripada mengedit unit bawaan package.
- `journalctl -u service` adalah titik awal terbaik saat service gagal start.
- Target seperti `multi-user.target` dan `graphical.target` menggantikan konsep runlevel lama.

### 2.6 Containers

Konsep:

- image: template read-only
- container: instance berjalan dari image
- registry: tempat image disimpan
- volume: persistent storage
- network: konektivitas antar container
- namespace: isolasi process/network/mount
- cgroups: resource limits

Penjelasan:

- Container menjalankan process yang diisolasi, tetapi tetap memakai kernel host.
- Image adalah artifact immutable yang berisi filesystem dan metadata untuk menjalankan aplikasi.
- Volume dipakai agar data tetap ada meskipun container dihapus.
- Port publishing seperti `-p 8080:80` menghubungkan port host ke port container.
- Environment variable sering dipakai untuk konfigurasi aplikasi container.
- Rootless container mengurangi dampak jika process di dalam container berhasil dieksploitasi.
- Jangan menyimpan secret langsung di image atau command history.

Docker/Podman commands:

```bash
# Mengelola image, container, log, volume, dan network container.
docker version

# Mengelola image, container, log, volume, dan network container.
docker pull nginx

# Mengelola image, container, log, volume, dan network container.
docker images

# Mengelola image, container, log, volume, dan network container.
docker run -d --name web -p 8080:80 nginx

# Mengelola image, container, log, volume, dan network container.
docker ps

# Mengelola image, container, log, volume, dan network container.
docker logs web

# Mengelola image, container, log, volume, dan network container.
docker exec -it web sh

# Mengelola image, container, log, volume, dan network container.
docker stop web

# Mengelola image, container, log, volume, dan network container.
docker rm web

# Mengelola image, container, log, volume, dan network container.
docker rmi nginx

# Mengelola image, container, log, volume, dan network container.
docker volume ls

# Mengelola image, container, log, volume, dan network container.
docker network ls

# Mengelola image, container, log, volume, dan network container.
docker network create appnet

# Mengelola image, container, log, volume, dan network container.
docker system prune
```

Podman:

```bash
# Mengelola image, container, log, volume, dan network container.
podman run -d --name web -p 8080:80 nginx

# Mengelola image, container, log, volume, dan network container.
podman ps

# Mengelola image, container, log, volume, dan network container.
podman logs web

# Mengelola image, container, log, volume, dan network container.
podman generate systemd --name web
```

Container network types:

- bridge
- host
- none
- overlay
- macvlan
- ipvlan

Penjelasan:

- `bridge` adalah mode default yang memberi network virtual terisolasi dengan NAT.
- `host` membuat container memakai network namespace host, sederhana tetapi isolasinya lebih rendah.
- `none` mematikan network container, cocok untuk workload lokal tertentu.
- `overlay` dipakai untuk network antar host di orchestrator.
- `macvlan` dan `ipvlan` membuat container tampak seperti host tersendiri di network fisik, tetapi perlu dukungan network yang sesuai.

Privileged vs unprivileged:

- privileged container punya akses lebih besar ke host
- rootless/unprivileged lebih aman untuk banyak use case

Troubleshooting container:

```bash
# Mengelola image, container, log, volume, dan network container.
docker inspect web

# Mengelola image, container, log, volume, dan network container.
docker logs web

# Melihat socket dan port yang sedang listen atau aktif.
ss -tulpn

# Menguji koneksi aplikasi atau port dari sisi client.
curl localhost:8080

# Mengelola image, container, log, volume, dan network container.
docker exec -it web env
```

---

## 3.0 Security

**Praktik setelah bab ini:** buktikan security posture dari permission, sudo, network exposure, dan firewall.

```bash
# Melihat privilege sudo untuk user saat ini.
sudo -l

# Mencari file SUID yang umum menjadi titik privilege risk.
find /usr -perm -4000 -type f -ls 2>/dev/null

# Melihat listening socket yang expose service.
ss -tulpn

# Melihat ruleset firewall nftables jika tersedia.
sudo nft list ruleset
```

Catat: command yang boleh dijalankan via sudo, file SUID, port terbuka, owner process, dan rule firewall yang relevan.

Bagian ini merangkum authentication, authorization, accounting, firewall, hardening, account security, cryptography, compliance, dan auditing.

### 3.1 Authorization, Authentication, Accounting

Konsep:

- authentication: membuktikan identitas
- authorization: menentukan akses
- accounting/auditing: mencatat aktivitas

Penjelasan:

- Authentication menjawab "siapa kamu?" melalui password, key, token, Kerberos ticket, certificate, atau MFA.
- Authorization menjawab "kamu boleh melakukan apa?" melalui permission, group, sudoers, ACL, SELinux, Polkit, atau policy aplikasi.
- Accounting/auditing menjawab "apa yang terjadi dan siapa yang melakukannya?" melalui log, auditd, dan session record.
- Masalah login tidak selalu berarti password salah. Bisa juga account expired, PAM rule gagal, shell invalid, home permission salah, atau identity backend tidak reachable.
- Saat troubleshooting akses, pisahkan masalah identity, authentication, authorization, dan environment login.

PAM:

```text
/etc/pam.d/
/etc/security/
```

PAM stack:

- `auth`
- `account`
- `password`
- `session`

Identity services:

- LDAP: directory service
- Kerberos: ticket-based authentication
- SSSD: client identity/authentication untuk LDAP/Kerberos/AD
- Winbind: integrasi Samba/Active Directory
- Samba: file sharing SMB/CIFS dan integrasi Windows
- Polkit: authorization framework untuk aksi privileged di Linux desktop/server

Penjelasan:

- PAM adalah framework modular yang dipakai banyak service login seperti `sshd`, `login`, `sudo`, dan display manager.
- NSS menentukan dari mana sistem mencari user, group, host, dan database lain, misalnya file lokal atau LDAP.
- LDAP menyimpan identity secara terpusat, tetapi biasanya perlu SSSD agar cache dan integrasi login lebih stabil.
- Kerberos memakai ticket sehingga password tidak perlu dikirim berulang ke setiap service.
- Samba/Winbind penting saat Linux harus berinteraksi dengan environment Windows atau Active Directory.
- Polkit sering muncul saat user non-root menjalankan aksi admin melalui desktop atau tool system service.

Command:

```bash
# Melihat identitas, group, login, atau database akun.
getent passwd user

# Melihat identitas, group, login, atau database akun.
id user

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
kinit user

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
klist

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
realm list

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
sssctl domain-list

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
sssctl user-checks user

# Menguji identity service seperti Kerberos, SSSD, realm, atau Samba.
pdbedit -L
```

File:

```text
/etc/sssd/sssd.conf
/etc/krb5.conf
/etc/samba/smb.conf
/etc/nsswitch.conf
```

### 3.2 Firewalls

Firewall mengontrol traffic network yang boleh masuk, keluar, atau diteruskan oleh host.

Konsep utama:

| Konsep | Arti |
|---|---|
| Rule | Aturan yang menentukan traffic diterima, ditolak, atau diteruskan |
| Chain | Kumpulan rule pada titik tertentu dalam packet flow |
| Table | Kelompok chain untuk fungsi tertentu |
| Zone | Model trust level pada `firewalld` |
| Service | Nama logis untuk port/protocol tertentu |
| Runtime vs permanent | Perubahan sementara vs perubahan yang bertahan setelah reload/reboot |

Gambaran mentalnya:

- Service listen di port tertentu.
- Firewall menentukan apakah traffic boleh mencapai service tersebut.
- Route menentukan apakah packet bisa sampai ke host.
- DNS hanya membantu menemukan IP, bukan membuka akses.

Kenapa ini penting:

- Port terbuka di aplikasi belum tentu terbuka di firewall.
- Firewall host bisa berbeda dari firewall cloud, router, security group, atau container network.
- Perubahan firewall di server remote bisa memutus akses admin jika SSH ikut terblokir.
- Selalu cek service listen dan firewall bersama-sama saat troubleshooting koneksi.

firewalld:

```bash
# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl enable --now firewalld

# Melihat atau mengubah aturan firewall.
firewall-cmd --state

# Melihat atau mengubah aturan firewall.
firewall-cmd --get-default-zone

# Melihat atau mengubah aturan firewall.
firewall-cmd --get-active-zones

# Melihat atau mengubah aturan firewall.
firewall-cmd --list-all

# Melihat atau mengubah aturan firewall.
sudo firewall-cmd --add-service=http

# Melihat atau mengubah aturan firewall.
sudo firewall-cmd --add-service=http --permanent

# Melihat atau mengubah aturan firewall.
sudo firewall-cmd --reload

# Melihat atau mengubah aturan firewall.
sudo firewall-cmd --add-port=8080/tcp --permanent

# Melihat atau mengubah aturan firewall.
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" service name="ssh" accept' --permanent
```

Concepts:

- runtime config hilang setelah reload/reboot
- permanent config bertahan
- zone menentukan trust level interface/source
- service memakai definisi port/protocol
- rich rule untuk rule lebih spesifik

Penjelasan:

- Firewall bekerja di jalur packet network. Service bisa aktif tetapi tetap tidak bisa diakses jika firewall menolak traffic.
- `firewalld` memakai konsep zone untuk membedakan trust level network, misalnya public, internal, atau trusted.
- Runtime rule cocok untuk testing cepat, permanent rule cocok untuk konfigurasi jangka panjang.
- `iptables` adalah tooling lama yang masih banyak ditemukan, sedangkan `nftables` adalah generasi lebih baru.
- `ufw` adalah frontend sederhana yang umum di Ubuntu.
- Saat mengubah firewall di remote server, pastikan akses SSH tidak terkunci. Gunakan session kedua atau scheduled rollback jika perlu.

Rich rules:

- Rich rule adalah rule `firewalld` yang lebih ekspresif daripada sekadar membuka service atau port.
- Dipakai saat rule perlu source address, log prefix, family IPv4/IPv6, service tertentu, port tertentu, atau action khusus.
- Contoh use case: hanya subnet admin yang boleh SSH, atau log traffic tertentu sebelum ditolak.

Masquerading dan forwarding:

- Forwarding berarti host meneruskan packet dari satu network/interface ke network/interface lain.
- Masquerading adalah bentuk NAT yang membuat traffic keluar terlihat berasal dari IP host firewall/router.
- Masquerading sering dipakai pada gateway kecil, lab NAT, container host, atau VM network.
- Jangan aktifkan forwarding/NAT tanpa memahami jalur traffic dan policy firewall karena bisa membuka akses yang tidak diinginkan.

ufw:

```bash
# Melihat atau mengubah aturan firewall.
sudo ufw status verbose

# Melihat atau mengubah aturan firewall.
sudo ufw allow ssh

# Melihat atau mengubah aturan firewall.
sudo ufw allow 80/tcp

# Melihat atau mengubah aturan firewall.
sudo ufw deny from 192.168.1.10

# Melihat atau mengubah aturan firewall.
sudo ufw enable

# Melihat atau mengubah aturan firewall.
sudo ufw disable
```

iptables:

```bash
# Melihat atau mengubah aturan firewall.
sudo iptables -L -n -v

# Melihat atau mengubah aturan firewall.
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Melihat atau mengubah aturan firewall.
sudo iptables -A INPUT -j DROP
```

nftables:

```bash
# Melihat atau mengubah aturan firewall.
sudo nft list ruleset

# Melihat atau mengubah aturan firewall.
sudo nft add table inet filter

# Melihat atau mengubah aturan firewall.
sudo nft add chain inet filter input '{ type filter hook input priority 0 ; }'
```

Troubleshooting:

```bash
# Melihat socket dan port yang sedang listen atau aktif.
ss -tulpn

# Menguji koneksi aplikasi atau port dari sisi client.
curl -v http://server:port

# Menguji koneksi aplikasi atau port dari sisi client.
nc -vz server port

# Melihat atau mengubah aturan firewall.
firewall-cmd --list-all

# Melihat atau mengubah aturan firewall.
sudo nft list ruleset
```

### 3.3 OS Hardening

Area utama:

- permission dan ownership benar
- `sudo` minimal privilege
- SSH hardened
- firewall aktif
- SELinux/AppArmor aktif
- service yang tidak perlu dimatikan
- package rutin update
- audit/logging aktif

Penjelasan:

- Hardening adalah proses mengurangi attack surface dan membatasi dampak jika ada komponen yang gagal.
- Service yang tidak dipakai sebaiknya dimatikan agar tidak membuka port, dependency, atau permission tambahan.
- SSH sebaiknya memakai key-based authentication, membatasi root login, dan membatasi user yang boleh login.
- Minimal privilege berarti user/service hanya mendapat akses yang diperlukan untuk tugasnya.
- SELinux/AppArmor menambah lapisan kontrol walaupun permission Unix terlihat benar.
- Patch management penting karena banyak kompromi terjadi dari vulnerability lama yang sebenarnya sudah punya fix.

#### SSH

SSH adalah protokol untuk login remote, menjalankan command remote, transfer file, dan tunneling secara terenkripsi.

Komponen utama:

| Komponen | Fungsi |
|---|---|
| SSH client | program yang dipakai dari sisi admin, biasanya `ssh` |
| SSH server | daemon di host tujuan, biasanya `sshd` |
| host key | identitas server SSH |
| user key | key pair untuk autentikasi user |
| `known_hosts` | daftar host key server yang pernah dipercaya client |
| `authorized_keys` | daftar public key yang boleh login ke akun user |

Cara kerja public key authentication:

1. User punya private key dan public key.
2. Public key dipasang di server pada `~/.ssh/authorized_keys`.
3. Private key tetap di client dan tidak boleh dibagikan.
4. Saat login, server memberi challenge yang hanya bisa dijawab oleh pemilik private key.
5. Jika cocok, login diterima tanpa mengirim password user.

File penting:

| File | Fungsi |
|---|---|
| `/etc/ssh/sshd_config` | konfigurasi server SSH |
| `/etc/ssh/ssh_config` | konfigurasi client global |
| `~/.ssh/config` | konfigurasi client per user |
| `~/.ssh/known_hosts` | host key yang dikenal client |
| `~/.ssh/authorized_keys` | public key yang boleh login ke user |
| `~/.ssh/id_ed25519` | private key user |
| `~/.ssh/id_ed25519.pub` | public key user |

`~/.ssh/config`:

```text
Host app
    HostName 192.168.1.10
    User deploy
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Field umum `~/.ssh/config`:

| Field | Contoh | Arti |
|---|---|---|
| `Host` | `app` | alias yang dipakai saat menjalankan `ssh app` |
| `HostName` | `192.168.1.10` | host/IP tujuan sebenarnya |
| `User` | `deploy` | user login default |
| `Port` | `22` | port SSH tujuan |
| `IdentityFile` | `~/.ssh/id_ed25519` | private key yang dipakai |

`/etc/ssh/sshd_config`:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers alice bob
Port 22
```

Directive umum `sshd_config`:

| Directive | Contoh | Arti |
|---|---|---|
| `PermitRootLogin` | `no` | mengatur apakah root boleh login langsung |
| `PasswordAuthentication` | `no` | mengatur login password |
| `PubkeyAuthentication` | `yes` | mengaktifkan login public key |
| `AllowUsers` | `alice bob` | membatasi user yang boleh login |
| `Port` | `22` | port SSH server |

Known Hosts:

- `known_hosts` menyimpan host key server yang pernah dihubungi client.
- Tujuannya mencegah man-in-the-middle attack.
- Jika host key berubah, SSH memberi warning karena server bisa saja diganti, reinstall, atau ada serangan.
- Jangan langsung menghapus warning host key tanpa memverifikasi fingerprint server.

Permission penting:

| Path | Permission umum |
|---|---|
| `~/.ssh` | `700` |
| `authorized_keys` | `600` |
| private key | `600` |
| public key | `644` |

Port forwarding:

| Jenis | Fungsi |
|---|---|
| local forwarding | akses service remote lewat port lokal |
| remote forwarding | expose service lokal lewat port di server remote |
| dynamic forwarding | membuat SOCKS proxy lewat SSH |

Menyalin public key ke server:

```bash
# Menyalin public key lokal ke authorized_keys user di server.
ssh-copy-id user@server

# Login ke server menggunakan SSH setelah key terpasang.
ssh user@server
```

Jika `ssh-copy-id` tidak tersedia, public key bisa ditambahkan manual ke:

```text
~/.ssh/authorized_keys
```

SSH hardening:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers alice bob
```

Command:

```bash
# Memvalidasi konfigurasi SSH server sebelum reload.
sudo sshd -t

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl reload sshd

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl status sshd
```

Service hardening:

```bash
# Mengelola atau memeriksa unit dan service systemd.
systemctl list-unit-files --state=enabled

# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl disable --now service
```

#### SELinux

SELinux adalah layer access control tambahan di atas permission Unix biasa. Permission Unix disebut DAC, sedangkan SELinux menerapkan MAC.

Perbandingan:

| Model | Arti | Contoh |
|---|---|---|
| DAC | Discretionary Access Control | owner file menentukan permission |
| MAC | Mandatory Access Control | policy sistem menentukan akses |

Mode SELinux:

| Mode | Arti |
|---|---|
| Enforcing | policy aktif dan denial benar-benar diblokir |
| Permissive | denial dicatat tetapi tidak diblokir |
| Disabled | SELinux tidak aktif |

Security context:

```text
user:role:type:level
```

Contoh security context:

```text
system_u:object_r:httpd_sys_content_t:s0
```

Field security context:

| Field | Contoh | Arti |
|---|---|---|
| user | `system_u` | SELinux user, bukan selalu sama dengan Linux user |
| role | `object_r` | role SELinux |
| type | `httpd_sys_content_t` | tipe yang paling sering menentukan access decision |
| level | `s0` | level MLS/MCS |

Bagian yang paling sering dipakai admin adalah `type`.

Contoh:

| Context type | Biasanya untuk |
|---|---|
| `httpd_t` | process Apache/httpd |
| `httpd_sys_content_t` | file web content read-only |
| `httpd_sys_rw_content_t` | file web content yang boleh ditulis |
| `ssh_port_t` | port SSH |
| `http_port_t` | port HTTP |

Cara berpikir troubleshooting SELinux:

1. Cek apakah SELinux enforcing.
2. Cek context file dengan `ls -Z`.
3. Cek context process dengan `ps -eZ`.
4. Cari denial di audit log.
5. Perbaiki label permanen dengan `semanage fcontext`, lalu apply dengan `restorecon`.
6. Gunakan boolean jika policy memang menyediakan toggle.
7. Jangan langsung membuat policy custom dari `audit2allow` tanpa memahami denial-nya.

SELinux:

```bash
# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
getenforce

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sestatus

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo setenforce 0

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo setenforce 1

# Menampilkan isi direktori atau file yang cocok.
ls -Z

# Melihat process dan penggunaan resource.
ps -eZ

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo restorecon -Rv /var/www/html

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo semanage fcontext -a -t httpd_sys_content_t '/web(/.*)?'

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo semanage port -a -t http_port_t -p tcp 8080

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
getsebool -a

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo setsebool -P httpd_can_network_connect on

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
ausearch -m avc -ts recent

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
audit2allow -a
```

AppArmor:

```bash
# Memeriksa atau mengubah mode profil AppArmor.
aa-status

# Memeriksa atau mengubah mode profil AppArmor.
sudo aa-enforce profile

# Memeriksa atau mengubah mode profil AppArmor.
sudo aa-complain profile
```

### 3.4 Account Security

Account security membahas cara melindungi akun user dan service account dari penyalahgunaan.

Hal yang perlu dipahami:

- password policy mengatur umur dan kualitas password
- lockout policy membatasi percobaan login gagal
- shell non-login mencegah akun service dipakai login interaktif
- MFA menambah faktor autentikasi di luar password
- review login membantu menemukan akses mencurigakan

Password policy:

```bash
# Mengelola akun user, group, shell, password, atau masa berlaku akun.
chage -l user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
sudo chage -M 90 -m 7 -W 14 user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
passwd -S user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
sudo passwd -l user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
sudo passwd -u user
```

Restrict shell:

```bash
# Mengelola akun user, group, shell, password, atau masa berlaku akun.
sudo usermod -s /usr/sbin/nologin serviceuser

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
sudo usermod -s /bin/false serviceuser
```

MFA:

- TOTP: time-based one-time password
- sering diintegrasikan lewat PAM
- pastikan ada recovery code/admin path sebelum enforce

Penjelasan:

- Account security mengatur lifecycle user: pembuatan akun, password policy, lockout, expiry, privilege, dan offboarding.
- Password aging membantu memastikan password lama diganti, tetapi terlalu agresif bisa membuat user memilih password lemah.
- Lockout policy mengurangi brute force, tetapi bisa menjadi denial-of-service jika threshold terlalu rendah.
- MFA menambah lapisan keamanan, terutama untuk akses remote dan akun privileged.
- Akun service sebaiknya tidak punya password login interaktif dan hanya diberi akses ke resource yang dibutuhkan.

Login review:

```bash
# Melihat identitas, group, login, atau database akun.
last

# Melihat identitas, group, login, atau database akun.
lastlog

# Memeriksa atau mengelola akun yang terkunci karena gagal login.
faillock

# Memeriksa atau mengelola akun yang terkunci karena gagal login.
sudo faillock --user user
```

### 3.5 Cryptography

Cryptography dipakai untuk menjaga integritas, kerahasiaan, dan keaslian data.

Bedakan konsep ini:

| Konsep | Fungsi |
|---|---|
| Hash | Mengecek integritas data |
| Encryption | Menyembunyikan isi data |
| Signature | Membuktikan data dibuat/ditandatangani pihak tertentu |
| Certificate | Mengikat identitas dengan public key |
| Key | Rahasia atau public material yang dipakai operasi crypto |

Hash:

```bash
# Menghitung hash untuk verifikasi integritas file.
sha256sum file

# Menghitung hash untuk verifikasi integritas file.
sha512sum file

# Menghitung hash untuk verifikasi integritas file.
md5sum file
```

OpenSSL:

```bash
# Mengelola operasi TLS, certificate, random data, atau crypto dasar.
openssl version

# Mengelola operasi TLS, certificate, random data, atau crypto dasar.
openssl rand -hex 32

# Mengelola operasi TLS, certificate, random data, atau crypto dasar.
openssl x509 -in cert.pem -text -noout

# Mengelola operasi TLS, certificate, random data, atau crypto dasar.
openssl req -newkey rsa:4096 -nodes -keyout key.pem -x509 -days 365 -out cert.pem

# Mengelola operasi TLS, certificate, random data, atau crypto dasar.
openssl s_client -connect example.com:443
```

GPG:

```bash
# Mengelola key, enkripsi, dekripsi, dan signature GPG.
gpg --list-keys

# Mengelola key, enkripsi, dekripsi, dan signature GPG.
gpg --gen-key

# Mengelola key, enkripsi, dekripsi, dan signature GPG.
gpg --encrypt --recipient user@example.com file

# Mengelola key, enkripsi, dekripsi, dan signature GPG.
gpg --decrypt file.gpg

# Mengelola key, enkripsi, dekripsi, dan signature GPG.
gpg --verify file.sig file
```

LUKS disk encryption:

```bash
# Membuat atau membuka encrypted volume LUKS.
sudo cryptsetup luksFormat /dev/sdb1

# Membuat atau membuka encrypted volume LUKS.
sudo cryptsetup open /dev/sdb1 securedata

# Membuat filesystem baru pada block device.
sudo mkfs.ext4 /dev/mapper/securedata

# Membuat atau membuka encrypted volume LUKS.
sudo cryptsetup close securedata
```

Certificates:

- certificate membuktikan identitas public key
- CA menandatangani certificate
- private key harus dilindungi
- expired certificate menyebabkan service TLS gagal

Penjelasan:

- Hash dipakai untuk memverifikasi integritas, bukan untuk menyembunyikan data.
- Encryption melindungi kerahasiaan data, baik saat disimpan maupun saat dikirim.
- Signature membuktikan integritas dan asal data jika public key dipercaya.
- TLS certificate bergantung pada trust chain dari CA ke certificate server.
- Private key yang bocor harus dianggap kompromi dan diganti, bukan sekadar dipindahkan.
- LUKS melindungi data saat disk offline atau dicuri, tetapi tidak melindungi data dari process yang sudah berjalan dengan akses sah.

### 3.6 Compliance, Integrity, and Auditing

Bagian ini membahas cara membuktikan dan memantau bahwa sistem berada dalam kondisi yang diharapkan.

Konsep utama:

- auditing mencatat aktivitas penting untuk investigasi
- integrity checking mendeteksi perubahan file tidak sah
- compliance scanning membandingkan konfigurasi dengan baseline
- patch management menjaga sistem tetap mendapat perbaikan security
- log rotation menjaga log tetap tersedia tanpa memenuhi disk

auditd:

```bash
# Mengelola atau memeriksa unit dan service systemd.
sudo systemctl enable --now auditd

# Mengelola rule audit dan membaca laporan audit.
auditctl -s

# Mengelola rule audit dan membaca laporan audit.
sudo auditctl -w /etc/passwd -p wa -k identity

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
ausearch -k identity

# Mengelola rule audit dan membaca laporan audit.
aureport
```

Rules:

```text
/etc/audit/rules.d/audit.rules
/etc/audit/audit.rules
```

Contoh audit rule:

```text
-w /etc/passwd -p wa -k identity
```

Field audit rule:

| Field | Contoh | Arti |
|---|---|---|
| `-w` | `/etc/passwd` | path yang diawasi |
| `-p` | `wa` | permission event: write dan attribute change |
| `-k` | `identity` | key/tag agar mudah dicari |

Permission audit:

| Huruf | Arti |
|---|---|
| `r` | read |
| `w` | write |
| `x` | execute |
| `a` | attribute change |

Integrity:

```bash
# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -Va

# Memeriksa integritas file atau compliance security.
debsums

# Memeriksa integritas file atau compliance security.
aide --init

# Memeriksa integritas file atau compliance security.
aide --check
```

Compliance/security scanning:

```bash
# Memeriksa integritas file atau compliance security.
oscap --help
```

Tools/concepts:

- AIDE: file integrity monitoring
- OpenSCAP: compliance and vulnerability scanning
- CIS benchmark: hardening baseline
- CVE: vulnerability identifier
- patch management: proses update terkontrol

Penjelasan:

- Logging mencatat kejadian operasional, sedangkan auditing mencatat aktivitas yang penting untuk security dan compliance.
- `auditd` bisa memonitor akses file sensitif, perubahan identity, command privileged, atau event kernel tertentu.
- File integrity monitoring seperti AIDE membantu mendeteksi perubahan tidak sah pada file penting.
- Compliance scanning membandingkan konfigurasi sistem dengan baseline tertentu.
- Patch management tidak hanya menjalankan update, tetapi juga mencakup testing, maintenance window, rollback, dan pencatatan perubahan.

Logging:

```bash
# Membaca log dari systemd journal.
journalctl

# Menguji logging, rsyslog, atau rotasi log.
logger "manual test log"

# Menguji logging, rsyslog, atau rotasi log.
rsyslogd -N1

# Menguji logging, rsyslog, atau rotasi log.
logrotate -d /etc/logrotate.conf
```

File:

```text
/var/log
/etc/rsyslog.conf
/etc/rsyslog.d/
/etc/logrotate.conf
/etc/logrotate.d/
```

Contoh logrotate rule:

```text
/var/log/app/*.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
    create 0640 app adm
}
```

Directive logrotate:

| Directive | Arti |
|---|---|
| `daily` | rotasi harian |
| `rotate 7` | simpan 7 file rotasi |
| `compress` | kompres log lama |
| `missingok` | tidak error jika file log tidak ada |
| `notifempty` | jangan rotasi file kosong |
| `create 0640 app adm` | buat file log baru dengan mode, user, group |

---

## 4.0 Automation, Orchestration, and Scripting

**Praktik setelah bab ini:** ubah command manual menjadi script kecil yang bisa diuji dan diulang.

```bash
# Mengecek syntax script bash tanpa menjalankannya.
bash -n script.sh

# Menjalankan script dengan trace agar setiap command terlihat.
bash -x script.sh

# Menampilkan environment variable yang sedang aktif.
env | sort
```

Catat: input, output, exit code, variable yang dipakai, error handling, dan bagian script yang harus dibuat idempotent.

Bagian ini merangkum shell scripting, Python dasar, automation/config management, Git, CI/CD, dan AI best practices.

### 4.1 Automation and Orchestration

Konsep:

- automation: menjalankan task berulang secara otomatis
- orchestration: mengoordinasikan banyak task/sistem
- idempotency: dijalankan berkali-kali tetap menghasilkan state yang sama
- agent-based: node menjalankan agent
- agentless: controller connect tanpa agent khusus
- IaC: infrastructure as code

Penjelasan:

- Automation mengurangi kerja manual berulang dan membuat hasil lebih konsisten.
- Orchestration mengatur urutan dan dependency antar task, misalnya update package, deploy config, restart service, lalu health check.
- Idempotency penting karena automation sering dijalankan berulang. Task yang idempotent tidak membuat perubahan baru jika state sudah benar.
- Agentless seperti Ansible lebih mudah mulai karena cukup SSH, tetapi tetap perlu inventory, credential, dan privilege yang rapi.
- Agent-based seperti Puppet cocok untuk enforcement state berkelanjutan pada banyak host.
- Infrastructure as Code membuat konfigurasi bisa direview, diuji, dan di-rollback seperti code aplikasi.

Ansible:

```bash
# Menjalankan automation Ansible terhadap host target.
ansible --version

# Menjalankan automation Ansible terhadap host target.
ansible all -i inventory -m ping

# Menjalankan automation Ansible terhadap host target.
ansible all -i inventory -m command -a "uptime"

# Menjalankan automation Ansible terhadap host target.
ansible-playbook -i inventory site.yml
```

Inventory:

```ini
[web]
web1 ansible_host=192.168.1.10
web2 ansible_host=192.168.1.11
```

Field inventory:

| Bagian | Contoh | Arti |
|---|---|---|
| group | `[web]` | nama kelompok host |
| host alias | `web1` | nama host di inventory |
| host variable | `ansible_host=192.168.1.10` | IP/hostname target sebenarnya |

Playbook:

```yaml
- name: Install web server
  hosts: web
  become: true
  tasks:
    - name: Install nginx
      package:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: true
```

Field playbook:

| Field | Contoh | Arti |
|---|---|---|
| `name` | `Install web server` | nama play atau task |
| `hosts` | `web` | group/host target |
| `become` | `true` | menjalankan task dengan privilege escalation |
| `tasks` | list task | daftar pekerjaan yang dijalankan |
| `package` | module | module untuk mengelola package |
| `service` | module | module untuk mengelola service |
| `state` | `present`, `started` | desired state |

Puppet:

- configuration management berbasis manifest
- umumnya agent-based
- mendeklarasikan desired state

CI/CD basics:

- CI: build/test otomatis saat perubahan code
- CD: deploy otomatis atau semi-otomatis
- pipeline: urutan job
- artifact: hasil build
- secret: credential yang harus dikelola aman

Penjelasan:

- CI/CD untuk admin Linux sering dipakai untuk validasi script, lint Ansible playbook, build image, dan deploy konfigurasi.
- Pipeline harus memisahkan build, test, review, deploy staging, dan deploy production.
- Secret tidak boleh ditulis langsung di repository. Gunakan secret manager atau variable terenkripsi dari platform CI/CD.
- Artifact harus bisa ditelusuri ke commit tertentu agar rollback dan audit lebih mudah.
- Deployment yang baik punya health check dan rollback plan.

### 4.2 Shell Scripting

Shell scripting dipakai untuk menggabungkan command Linux menjadi workflow yang bisa diulang.

Konsep utama:

| Konsep | Arti |
|---|---|
| Script | File berisi urutan command |
| Interpreter | Program yang membaca dan menjalankan script, misalnya Bash |
| Variable | Tempat menyimpan nilai sementara |
| Exit code | Status berhasil/gagal dari command |
| Conditional | Percabangan berdasarkan kondisi |
| Loop | Pengulangan command |
| Function | Blok logic yang bisa dipakai ulang |

Gambaran mentalnya:

- Bash bagus untuk mengorkestrasi command Linux.
- Bash kurang cocok untuk struktur data kompleks, parsing berat, atau logic aplikasi besar.
- Script yang baik harus aman saat input kosong, path mengandung spasi, command gagal, atau environment berbeda.
- Banyak bug Bash berasal dari variable tidak di-quote, asumsi `PATH`, dan tidak mengecek exit code.

Kapan Bash cocok:

- automation kecil
- backup sederhana
- maintenance task
- glue antar command
- parsing log ringan

Kapan mulai pindah ke Python:

- logic bercabang banyak
- parsing JSON/YAML/API
- butuh error handling rapi
- butuh struktur data kompleks
- script mulai sulit dibaca

Shebang:

```bash
#!/usr/bin/env bash
```

Penjelasan:

- Shebang menentukan interpreter yang menjalankan script.
- `#!/usr/bin/env bash` mencari `bash` dari `PATH`, lebih portable di beberapa environment.
- Script admin harus aman terhadap spasi dalam path, input kosong, command gagal, dan environment yang berbeda.
- Quote variable seperti `"$var"` untuk menghindari word splitting dan globbing tidak sengaja.
- Gunakan `shellcheck` untuk menemukan bug umum sebelum script dipakai di mesin penting.

Variables:

```bash
# Mengatur nilai variabel untuk dipakai oleh command berikutnya.
name="server1"

# Mencetak nilai atau teks ke terminal.
echo "$name"

# Mencetak nilai atau teks ke terminal.
echo "${name}"
```

Arguments:

```bash
# Mencetak nilai atau teks ke terminal.
echo "$0"

# Mencetak nilai atau teks ke terminal.
echo "$1"

# Mencetak nilai atau teks ke terminal.
echo "$@"

# Mencetak nilai atau teks ke terminal.
echo "$#"
```

Exit:

```bash
# Mengakhiri script dengan status berhasil.
exit 0

# Mengakhiri script dengan status gagal.
exit 1
```

Safer defaults:

```bash
# Mengaktifkan mode Bash yang lebih aman untuk script.
set -euo pipefail
```

Expansion:

```bash
# Mencetak nilai atau teks ke terminal.
echo "${HOME}"

# Mencetak nilai atau teks ke terminal.
echo "$(date)"

# Mencetak nilai atau teks ke terminal.
echo `date`

# Menjalankan command di subshell tanpa mengubah direktori shell utama.
(cd /tmp && pwd)
```

IFS/OFS:

```bash
# Mengatur nilai variabel untuk dipakai oleh command berikutnya.
old_ifs="$IFS"

# Mengatur nilai variabel untuk dipakai oleh command berikutnya.
IFS=:

# Membaca satu baris input dan memecahnya ke beberapa variabel.
read -r user pass uid gid rest < /etc/passwd

# Mengatur nilai variabel untuk dipakai oleh command berikutnya.
IFS="$old_ifs"
```

Condition:

```bash
# Mengecek kondisi sebelum menjalankan aksi di dalam blok if.
if [ -f /etc/passwd ]; then

  # Mencetak nilai atau teks ke terminal.
  echo "exists"

  # Menutup blok if.
fi
```

Tests:

```bash
# Mengecek apakah path adalah direktori.
[ -d /etc ]

# Mengecek apakah path adalah regular file.
[ -f /etc/passwd ]

# Mengecek apakah variabel tidak kosong.
[ -n "$var" ]

# Mengecek apakah variabel kosong.
[ -z "$var" ]

# Membandingkan dua string.
[ "$a" = "$b" ]

# Membandingkan angka dengan operator numerik.
[ "$n" -eq 1 ]
```

Comparisons:

- numeric: `-eq`, `-ne`, `-gt`, `-ge`, `-lt`, `-le`
- string: `=`, `==`, `!=`, `<`, `>`

Case:

```bash
# Memilih aksi berdasarkan nilai input atau argumen.
case "$1" in

  # Menangani salah satu cabang pilihan pada struktur case.
  start) echo start ;;

  # Menangani salah satu cabang pilihan pada struktur case.
  stop) echo stop ;;

  # Menangani salah satu cabang pilihan pada struktur case.
  *) echo "usage: $0 start|stop" ;;

# Menutup struktur case.
esac
```

Loops:

```bash
# Melakukan iterasi terhadap daftar item.
for file in *.log; do

  # Mencetak nilai atau teks ke terminal.
  echo "$file"

# Menutup loop for.
done

# Menjalankan loop selama kondisi masih terpenuhi.
while read -r line; do

  # Mencetak nilai atau teks ke terminal.
  echo "$line"

# Menutup loop while dan mengarahkan isi file sebagai input.
done < file

# Menjalankan loop sampai kondisi berhasil terpenuhi.
until ping -c1 host; do

  # Menunggu 5 detik sebelum iterasi berikutnya.
  sleep 5

# Menutup loop until.
done
```

Function:

```bash
# Mendefinisikan function shell agar aksi bisa dipakai ulang.
log() {

  # Mencetak nilai atau teks ke terminal.
  echo "$(date -Is) $*"
}
```

Regex:

```bash
# Mengecek kondisi sebelum menjalankan aksi di dalam blok if.
if [[ "$name" =~ ^web[0-9]+$ ]]; then

  # Mencetak nilai atau teks ke terminal.
  echo "web host"

  # Menutup blok if.
fi
```

Debug:

```bash
# Memvalidasi atau melakukan debugging script shell.
bash -n script.sh

# Memvalidasi atau melakukan debugging script shell.
bash -x script.sh

# Memvalidasi atau melakukan debugging script shell.
shellcheck script.sh
```

### 4.3 Python Basics for Linux Admins

Python berguna untuk pekerjaan administrasi Linux yang butuh logic lebih jelas daripada Bash.

Konsep utama:

| Konsep | Arti |
|---|---|
| Interpreter | Program `python3` yang menjalankan script Python |
| Virtual environment | Environment dependency terpisah dari Python sistem |
| Module | File/library Python yang bisa di-import |
| Standard library | Library bawaan Python |
| Subprocess | Cara Python menjalankan command sistem |
| Exception | Mekanisme error handling Python |

Gambaran mentalnya:

- Bash kuat untuk menjalankan command, Python kuat untuk mengelola data dan logic.
- Python cocok untuk membaca file, parsing JSON, memanggil API, membuat report, atau menjalankan command dengan validasi.
- Jangan sembarang memakai `shell=True` karena bisa membuka risiko command injection.
- Gunakan virtual environment agar dependency script tidak merusak package Python sistem.
- Script Python admin sebaiknya punya logging dan exit code yang jelas.

Run:

```bash
# Menjalankan Python atau mengelola package Python.
python3 --version

# Menjalankan Python atau mengelola package Python.
python3 script.py
```

Virtual environment:

```bash
# Menjalankan Python atau mengelola package Python.
python3 -m venv .venv

# Memuat environment shell dari file atau virtual environment.
source .venv/bin/activate

# Menjalankan Python atau mengelola package Python.
pip install requests

# Menjalankan Python atau mengelola package Python.
pip freeze
```

Basic script:

```python
#!/usr/bin/env python3
import sys
from pathlib import Path

path = Path(sys.argv[1])
print(path.exists())
```

Subprocess:

```python
#!/usr/bin/env python3
import subprocess

result = subprocess.run(["systemctl", "is-active", "sshd"], capture_output=True, text=True)
print(result.stdout.strip())
```

Data types:

- string
- integer
- float
- boolean
- list
- dict
- tuple
- set

Penjelasan:

- Python cocok ketika logic mulai kompleks, perlu parsing JSON/YAML/API, atau butuh error handling yang lebih rapi daripada Bash.
- Gunakan `subprocess.run()` dengan list argumen agar lebih aman daripada membangun command string.
- `pathlib` membantu mengelola path secara lebih jelas daripada manipulasi string manual.
- Virtual environment mencegah dependency project mengganggu Python sistem.
- Logging Python lebih cocok daripada `print` untuk script yang akan dipakai rutin atau dijalankan cron/systemd.

JSON:

```python
import json

data = {"service": "sshd", "enabled": True}
print(json.dumps(data))
```

Logging:

```python
import logging

logging.basicConfig(level=logging.INFO)
logging.info("started")
```

### 4.4 Version Control with Git

Git dipakai untuk melacak perubahan file, terutama script, konfigurasi, dokumentasi, dan automation.

Konsep utama:

| Konsep | Arti |
|---|---|
| Repository | Folder project yang dilacak Git |
| Working tree | File yang sedang kamu edit |
| Staging area | Area persiapan sebelum commit |
| Commit | Snapshot perubahan dengan pesan |
| Branch | Jalur kerja terpisah |
| Merge | Menggabungkan branch |
| Tag | Penanda versi tertentu |

Gambaran mentalnya:

- Kamu mengubah file di working tree.
- Kamu memilih perubahan yang ingin dimasukkan dengan `git add`.
- Kamu menyimpan snapshot dengan `git commit`.
- Kamu bisa melihat perubahan dengan `git diff`.
- Kamu bisa bekerja di branch agar eksperimen tidak mengganggu branch utama.

Kenapa ini penting untuk catatan dan kerja Linux:

- Perubahan script bisa dilacak dan di-rollback.
- Konfigurasi bisa direview sebelum diterapkan.
- Catatan belajar punya riwayat perkembangan.
- Tag bisa menandai versi stabil dari script atau dokumentasi.

Workflow:

```bash
# Mengelola version control, branch, commit, merge, atau tag.
git status

# Mengelola version control, branch, commit, merge, atau tag.
git add file

# Mengelola version control, branch, commit, merge, atau tag.
git commit -m "message"

# Mengelola version control, branch, commit, merge, atau tag.
git log --oneline

# Mengelola version control, branch, commit, merge, atau tag.
git diff

# Mengelola version control, branch, commit, merge, atau tag.
git branch

# Mengelola version control, branch, commit, merge, atau tag.
git switch -c feature

# Mengelola version control, branch, commit, merge, atau tag.
git merge feature

# Mengelola version control, branch, commit, merge, atau tag.
git pull

# Mengelola version control, branch, commit, merge, atau tag.
git push
```

Tags:

```bash
# Mengelola version control, branch, commit, merge, atau tag.
git tag

# Mengelola version control, branch, commit, merge, atau tag.
git tag v1.0.0

# Mengelola version control, branch, commit, merge, atau tag.
git tag -a v1.0.0 -m "release v1.0.0"

# Mengelola version control, branch, commit, merge, atau tag.
git push origin v1.0.0
```

Best practices:

- commit kecil dan jelas
- jangan commit secrets
- review diff sebelum commit
- gunakan branch untuk perubahan besar

Penjelasan:

- Git membuat perubahan konfigurasi, script, dan dokumentasi bisa dilacak.
- Commit kecil memudahkan review dan rollback.
- Branch membantu mengerjakan eksperimen tanpa mengganggu versi utama.
- Tag berguna untuk menandai release script, baseline konfigurasi, atau versi dokumentasi.
- Sebelum commit, jalankan `git diff` untuk memastikan tidak ada secret, file sementara, atau perubahan tidak sengaja.

### 4.5 AI Best Practices

AI/code generation bisa membantu admin Linux, tapi harus dipakai dengan kontrol.

Prinsip:

- jangan paste secret, token, private key, atau data sensitif
- minta penjelasan risiko sebelum menjalankan command destructive
- validasi command dengan `man`, docs, atau lab VM
- jalankan perubahan besar di staging dulu
- baca script hasil AI sebelum dieksekusi
- gunakan Git untuk review dan rollback
- minta test plan, bukan hanya script
- hindari prompt yang ambigu untuk operasi production

Penjelasan:

- AI bisa mempercepat drafting script, menjelaskan log, atau memberi checklist troubleshooting, tetapi tetap perlu validasi manusia.
- Output AI bisa salah konteks distro, versi tool, path file, atau asumsi privilege.
- Untuk command berisiko, minta mode dry-run, backup plan, dan rollback plan.
- Jangan menjalankan script panjang tanpa membaca alurnya, terutama jika ada `rm`, `mkfs`, `dd`, redirect overwrite, firewall change, atau perubahan user/sudo.
- Simpan prompt dan hasil yang dipakai untuk perubahan besar agar keputusan bisa diaudit kembali.

Contoh prompt yang lebih aman:

```text
Buatkan script bash untuk audit disk usage tanpa menghapus file.
Jelaskan command yang dipakai dan risiko masing-masing.
```

---

## 5.0 Troubleshooting

**Praktik setelah bab ini:** mulai dari gejala, lalu kumpulkan bukti OS, resource, log, dan network.

```bash
# Melihat log kernel terbaru.
dmesg -T | tail -n 50

# Melihat error prioritas tinggi dari journal.
journalctl -p err --no-pager -n 50

# Melihat disk usage filesystem.
df -h

# Melihat memory dan swap.
free -h
```

Catat: waktu kejadian, error pertama, resource yang penuh, service terdampak, dan command validasi setelah perbaikan.

Bagian ini merangkum monitoring, troubleshooting hardware/storage/OS, network, security, dan performance.

### 5.1 System Monitoring

Monitoring adalah proses membaca kondisi sistem secara rutin agar masalah terlihat sebelum menjadi outage.

Konsep utama:

| Konsep | Arti |
|---|---|
| Metric | Angka terukur seperti CPU, memory, disk, latency |
| Log | Catatan event yang menjelaskan apa yang terjadi |
| Alert | Notifikasi saat kondisi melewati batas tertentu |
| Baseline | Kondisi normal sistem sebagai pembanding |
| Trend | Pola perubahan dari waktu ke waktu |
| Health check | Pemeriksaan apakah service masih sehat |

Gambaran mentalnya:

- Metric menjawab "berapa besar/kecil kondisinya?"
- Log menjawab "apa yang terjadi?"
- Alert menjawab "kapan perlu perhatian?"
- Baseline menjawab "apakah kondisi ini normal untuk sistem ini?"

Kenapa ini penting:

- CPU 80% bisa normal untuk batch job, tetapi berbahaya untuk service latency-sensitive.
- Disk 90% bisa aman sementara, tetapi berisiko jika growth cepat.
- Memory penuh belum tentu buruk di Linux karena page cache memang memakai memory kosong.
- Tanpa baseline, angka monitoring mudah disalahartikan.

Health command:

```bash
# Membaca metrik performa CPU, memory, load, atau I/O.
uptime

# Melihat process dan penggunaan resource.
top

# Melihat process dan penggunaan resource.
htop

# Membaca metrik performa CPU, memory, load, atau I/O.
free -h

# Membaca metrik performa CPU, memory, load, atau I/O.
vmstat 1

# Membaca metrik performa CPU, memory, load, atau I/O.
iostat -xz 1

# Membaca metrik performa CPU, memory, load, atau I/O.
mpstat 1

# Membaca metrik performa CPU, memory, load, atau I/O.
pidstat 1

# Menampilkan penggunaan kapasitas filesystem.
df -h

# Menampilkan penggunaan kapasitas filesystem.
df -i

# Melihat socket dan port yang sedang listen atau aktif.
ss -tulpn

# Membaca log dari systemd journal.
journalctl -p err
```

Logs:

```bash
# Membaca log dari systemd journal.
journalctl -xe

# Membaca log dari systemd journal.
journalctl -b

# Membaca log dari systemd journal.
journalctl -u service

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/syslog

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/messages

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/auth.log

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/secure
```

Monitoring concepts:

- threshold
- alert
- event
- notification
- log aggregation
- health check
- webhook
- SNMP
- SNMP traps
- MIB

Penjelasan:

- Monitoring membantu melihat kondisi sistem sebelum user melaporkan masalah.
- Metric menunjukkan angka seperti CPU, memory, disk usage, latency, atau error rate.
- Log menjelaskan event dan konteks yang tidak selalu terlihat dari metric.
- Threshold adalah batas yang memicu perhatian, misalnya disk usage di atas 85%.
- Alert harus actionable. Alert yang terlalu banyak dan tidak jelas akan diabaikan.
- SNMP umum pada network device, UPS, storage appliance, dan beberapa server hardware.
- MIB mendefinisikan object yang bisa dibaca SNMP, sedangkan trap adalah notifikasi event dari device ke monitoring system.

Data acquisition:

- agent-based monitoring
- agentless monitoring
- logs
- metrics
- tracing
- synthetic checks

Penjelasan:

- Agent-based monitoring memasang agent di host, biasanya memberi data lebih detail.
- Agentless monitoring lebih sederhana dari sisi host, tetapi data bisa lebih terbatas.
- Synthetic check meniru perilaku user atau client, misalnya HTTP request berkala ke endpoint aplikasi.
- Tracing membantu melihat perjalanan request antar service, terutama di sistem microservices.
- Gabungan metric, log, dan tracing memberi gambaran yang lebih lengkap daripada satu sumber saja.

### 5.2 Hardware, Storage, and OS Issues

Common issues:

- kernel panic
- data corruption
- kernel corruption
- package dependency issue
- filesystem will not mount
- server not turning on
- OS filesystem full
- server inaccessible
- device failure
- inode exhaustion
- partition not writable
- segmentation fault
- GRUB misconfiguration
- killed process
- PATH misconfiguration
- systemd unit failure
- missing or disabled driver
- unresponsive process
- quota issue
- memory leak

Penjelasan:

- Troubleshooting yang baik dimulai dari gejala, waktu kejadian, scope, dan perubahan terakhir.
- Jangan langsung memperbaiki sebelum mengumpulkan bukti minimum seperti log, status service, disk usage, dan recent changes.
- Kernel panic biasanya butuh analisis log kernel, hardware, driver, atau crash dump.
- Data corruption bisa berasal dari storage rusak, power loss, bug aplikasi, atau filesystem error.
- Memory leak terlihat dari penggunaan memory process yang terus naik dan tidak turun setelah workload normal.
- PATH misconfiguration sering membuat script gagal karena command tidak ditemukan ketika dijalankan dari cron atau systemd.
- Untuk masalah berat, ambil snapshot atau backup state sebelum mencoba recovery agresif.

Boot issue workflow:

```bash
# Membaca log dari systemd journal.
journalctl -b -1

# Mengelola atau memeriksa unit dan service systemd.
systemctl --failed

# Membaca isi file langsung ke terminal.
cat /proc/cmdline

# Menampilkan struktur block device, partition, dan mount point.
lsblk

# Menampilkan UUID, label, dan tipe filesystem.
blkid

# Menampilkan atau mengelola mount filesystem.
mount

# Menampilkan atau mengelola mount filesystem.
findmnt
```

GRUB recovery:

- boot ke rescue mode
- cek `/boot`
- cek `/etc/fstab`
- rebuild GRUB config
- rebuild initramfs bila driver/storage berubah

Filesystem will not mount:

```bash
# Menampilkan struktur block device, partition, dan mount point.
lsblk -f

# Menampilkan UUID, label, dan tipe filesystem.
blkid

# Menampilkan atau mengelola mount filesystem.
sudo mount -v /mountpoint

# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | tail

# Memeriksa dan memperbaiki filesystem.
sudo fsck /dev/device
```

Disk full:

```bash
# Menampilkan penggunaan kapasitas filesystem.
df -h

# Menampilkan penggunaan kapasitas filesystem.
df -i

# Menghitung ukuran file atau direktori.
du -xhd1 /

# Melihat file atau socket yang sedang dibuka process.
lsof +L1

# Membaca log dari systemd journal.
journalctl --disk-usage

# Membaca log dari systemd journal.
sudo journalctl --vacuum-time=7d
```

Package dependency:

```bash
# Mengelola package dan repository pada distro Debian/Ubuntu.
sudo apt -f install

# Mengelola package DEB tingkat rendah.
sudo dpkg --configure -a

# Mengelola package dan repository pada distro RPM-based.
sudo dnf check

# Mengelola package dan repository pada distro RPM-based.
sudo dnf distro-sync
```

Driver issue:

```bash
# Menampilkan perangkat PCI yang terdeteksi.
lspci -k

# Menampilkan perangkat USB yang terdeteksi.
lsusb

# Menampilkan kernel module yang sedang dimuat.
lsmod

# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | grep -i firmware

# Melihat metadata dan parameter kernel module.
modinfo module

# Memuat atau melepas kernel module beserta dependency.
sudo modprobe module
```

Killed process/OOM:

```bash
# Membaca log dari systemd journal.
journalctl -k | grep -i kill

# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | grep -i oom

# Membaca metrik performa CPU, memory, load, atau I/O.
free -h

# Melihat process dan penggunaan resource.
ps aux --sort=-%mem | head
```

Segmentation fault:

```bash
# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | tail

# Membaca log dari systemd journal.
journalctl -xe

# Melihat atau mengatur batas core dump untuk debugging crash.
ulimit -c

# Melacak system call untuk debugging process.
strace command
```

### 5.3 Networking Issues

Troubleshooting order:

1. Link/interface
2. IP address
3. Route/default gateway
4. DNS
5. Local listening service
6. Firewall
7. Remote path

Penjelasan:

- Urutan ini mencegah lompat ke dugaan yang terlalu jauh.
- Jika interface down, DNS dan firewall belum relevan.
- Jika ping ke IP berhasil tetapi nama domain gagal, fokus ke DNS.
- Jika service listen di `127.0.0.1`, client remote tidak akan bisa masuk walaupun firewall terbuka.
- Jika local curl berhasil tetapi remote gagal, cek firewall host, firewall network, route, NAT, dan security group.
- Packet capture seperti `tcpdump` dipakai ketika command biasa tidak cukup menunjukkan di mana packet berhenti.

Commands:

```bash
# Melihat atau mengubah konfigurasi network Linux.
ip link

# Melihat atau mengubah konfigurasi network Linux.
ip addr

# Melihat atau mengubah konfigurasi network Linux.
ip route

# Melihat atau mengubah konfigurasi network Linux.
ip neigh

# Menguji konektivitas dasar ke host tujuan.
ping gateway

# Menguji konektivitas dasar ke host tujuan.
ping 8.8.8.8

# Menguji konektivitas dasar ke host tujuan.
ping example.com

# Menguji DNS dan resolver configuration.
dig example.com

# Menguji DNS dan resolver configuration.
resolvectl status

# Melihat socket dan port yang sedang listen atau aktif.
ss -tulpn

# Menguji koneksi aplikasi atau port dari sisi client.
curl -v http://localhost:port

# Menguji koneksi aplikasi atau port dari sisi client.
curl -v http://server:port

# Melacak jalur network menuju host tujuan.
tracepath server

# Melacak jalur network menuju host tujuan.
traceroute server

# Menangkap packet network untuk analisis.
tcpdump -i eth0 port 80
```

Common issues:

- misconfigured firewall
- DHCP issue
- DNS issue
- wrong default route
- interface down
- MTU mismatch
- bonding misconfiguration
- MAC spoofing issue
- service listen only on loopback
- duplicate IP

Penjelasan:

- DHCP issue bisa membuat host tidak mendapat IP, gateway, DNS, atau lease yang benar.
- Duplicate IP menyebabkan koneksi tidak stabil dan gejala sering terlihat acak.
- MTU mismatch sering terlihat saat koneksi kecil berhasil tetapi transfer besar gagal.
- Bonding misconfiguration bisa muncul setelah perubahan switch, LACP, atau mode bond.
- Firewall yang salah bisa berada di host, network appliance, cloud security group, atau container layer.

MTU:

```bash
# Melihat atau mengubah konfigurasi network Linux.
ip link show

# Menguji konektivitas dasar ke host tujuan.
ping -M do -s 1472 host

# Melihat atau mengubah konfigurasi network Linux.
sudo ip link set dev eth0 mtu 1400
```

Bonding:

```bash
# Membaca isi file langsung ke terminal.
cat /proc/net/bonding/bond0

# Mengelola koneksi network melalui NetworkManager.
nmcli connection show
```

Firewall:

```bash
# Melihat atau mengubah aturan firewall.
firewall-cmd --list-all

# Melihat atau mengubah aturan firewall.
sudo nft list ruleset

# Melihat atau mengubah aturan firewall.
sudo iptables -L -n -v

# Melihat atau mengubah aturan firewall.
sudo ufw status verbose
```

### 5.4 Security Issues

Security issue biasanya muncul sebagai akses ditolak, login gagal, service tidak bisa membaca file, atau koneksi ditolak walaupun service terlihat aktif.

Cara berpikirnya:

- cek identity: user yang dipakai benar atau tidak
- cek authentication: user berhasil membuktikan identitas atau tidak
- cek authorization: user punya izin atau tidak
- cek filesystem permission: owner, group, mode, ACL, parent directory
- cek policy tambahan: SELinux, AppArmor, sudoers, PAM, firewall
- cek log: jangan menebak jika log sudah menjelaskan alasan penolakan

Permission issue:

```bash
# Memeriksa permission setiap komponen path.
namei -l /path/to/file

# Menampilkan isi direktori atau file yang cocok.
ls -l /path/to/file

# Melihat atau mengubah Access Control List file.
getfacl /path/to/file

# Melihat identitas, group, login, atau database akun.
id user

# Melihat hak sudo milik user tertentu.
sudo -l -U user
```

SELinux issue:

```bash
# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
getenforce

# Menampilkan isi direktori atau file yang cocok.
ls -Z /path

# Melihat process dan penggunaan resource.
ps -eZ | grep service

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
ausearch -m avc -ts recent

# Memeriksa atau memperbaiki konfigurasi dan denial SELinux.
sudo restorecon -Rv /path
```

Authentication issue:

```bash
# Membaca sebagian isi file dengan praktis.
tail -f /var/log/auth.log

# Membaca sebagian isi file dengan praktis.
tail -f /var/log/secure

# Membaca log dari systemd journal.
journalctl -u sshd

# Melihat identitas, group, login, atau database akun.
getent passwd user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
chage -l user

# Mengelola akun user, group, shell, password, atau masa berlaku akun.
passwd -S user

# Memeriksa atau mengelola akun yang terkunci karena gagal login.
faillock --user user
```

SSH issue:

```bash
# Menguji koneksi SSH dan detail proses autentikasi.
ssh -vvv user@host

# Memvalidasi konfigurasi SSH server sebelum reload.
sudo sshd -t

# Membaca log dari systemd journal.
sudo journalctl -u sshd

# Menampilkan isi direktori atau file yang cocok.
ls -ld ~/.ssh

# Menampilkan isi direktori atau file yang cocok.
ls -l ~/.ssh/authorized_keys
```

Expected SSH key permissions:

```text
~/.ssh                  700
~/.ssh/authorized_keys  600
private key             600
```

Penjelasan:

- Security troubleshooting harus memisahkan "ditolak karena identitas", "ditolak karena permission", dan "ditolak karena policy".
- Untuk file access, cek permission semua parent directory dengan `namei -l`, bukan hanya file akhirnya.
- Untuk SSH key, permission terlalu longgar bisa membuat OpenSSH menolak key.
- Untuk SELinux, permission Unix bisa terlihat benar tetapi access tetap ditolak karena context salah.
- Untuk sudo, user bisa ada di group yang benar tetapi policy sudoers belum cocok atau session belum membaca membership baru.
- Log authentication biasanya ada di `/var/log/auth.log`, `/var/log/secure`, atau `journalctl -u sshd` tergantung distro.

Vulnerability and compliance:

```bash
# Mengelola atau memverifikasi package RPM tingkat rendah.
rpm -Va

# Memeriksa integritas file atau compliance security.
aide --check

# Memeriksa integritas file atau compliance security.
oscap xccdf eval ...
```

### 5.5 Performance Issues

Performance issue berarti sistem masih berjalan, tetapi tidak secepat atau sestabil yang diharapkan.

Urutan berpikir:

1. Tentukan gejala: lambat, timeout, error, hang, atau resource penuh.
2. Tentukan scope: satu process, satu host, satu service, atau semua user.
3. Bandingkan dengan baseline normal.
4. Cari bottleneck utama: CPU, memory, disk I/O, network, lock, atau dependency remote.
5. Validasi dengan metric dan log, bukan hanya perasaan.

CPU:

```bash
# Melihat process dan penggunaan resource.
top

# Melihat process dan penggunaan resource.
htop

# Membaca metrik performa CPU, memory, load, atau I/O.
mpstat 1

# Membaca metrik performa CPU, memory, load, atau I/O.
pidstat -u 1

# Melihat process dan penggunaan resource.
ps aux --sort=-%cpu | head
```

Memory:

```bash
# Membaca metrik performa CPU, memory, load, atau I/O.
free -h

# Membaca metrik performa CPU, memory, load, atau I/O.
vmstat 1

# Membaca metrik performa CPU, memory, load, atau I/O.
pidstat -r 1

# Melihat process dan penggunaan resource.
ps aux --sort=-%mem | head

# Membaca pesan kernel, terutama error hardware dan driver.
dmesg | grep -i oom
```

Disk I/O:

```bash
# Membaca metrik performa CPU, memory, load, atau I/O.
iostat -xz 1

# Melihat process yang menggunakan disk I/O tinggi.
iotop

# Membaca metrik performa CPU, memory, load, atau I/O.
pidstat -d 1

# Menampilkan penggunaan kapasitas filesystem.
df -h

# Melakukan benchmark I/O storage.
fio --name=test --filename=/tmp/fio.test --size=100M --rw=readwrite
```

Network:

```bash
# Melihat socket dan port yang sedang listen atau aktif.
ss -s

# Melihat atau mengubah konfigurasi network Linux.
ip -s link

# Membaca metrik performa CPU, memory, load, atau I/O.
sar -n DEV 1

# Menangkap packet network untuk analisis.
tcpdump -i eth0
```

Load average:

- load tinggi + CPU tinggi: CPU bottleneck
- load tinggi + CPU idle: sering I/O wait
- memory habis: swap tinggi, OOM, aplikasi lambat

Penjelasan:

- Performance troubleshooting harus mencari bottleneck utama: CPU, memory, disk I/O, network, lock contention, atau external dependency.
- Load average bukan persen CPU. Load menunjukkan jumlah task yang running atau menunggu resource tertentu.
- CPU bottleneck terlihat dari CPU usage tinggi dan run queue tinggi.
- I/O bottleneck sering terlihat dari `iowait`, latency disk tinggi, atau process banyak dalam state `D`.
- Memory pressure terlihat dari swap aktif, OOM kill, page cache menyusut, atau aplikasi lambat saat alokasi memory.
- Network bottleneck bisa berasal dari bandwidth, packet loss, DNS latency, retransmit, atau service remote lambat.
- Selalu bandingkan kondisi sekarang dengan baseline normal jika tersedia.

---
