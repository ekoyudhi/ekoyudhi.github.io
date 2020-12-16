---
layout: post
title: Menghapus Startup Program melalui Registry Editor
categories: computer
---

Ada kalanya komputer kita menjalankan program secara otomatis pada saat kali pertama menyala. Proses tersebut pada komputer dinamakan Startup Program. Startup Program dapat dinonaktifkan melalui Start Menu -> Startup Apps. Selain menggunakan cara tersebut dapat pula menggunakan cara mengubah nilai pada registry windows melalui Regedit. Regedit dapat dijalankan melalui Start Menu -> Regedit. Nilai-nilai registry windows yang perlu dihapus agar dapat menonaktifkan Startup Program adalah sebagai berikut.

### Tahap 1 Untuk menghapus hanya pada "Current User"

```sh
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run32
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder
```

### Tahap 2 Untuk menghapus pada "All User"

Untuk Windows 10 32-bit dan 64-bit
```sh
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run32
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder
```
Apabila ada penambahan dari "Group Policy"
```sh
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
```
Tambahan untuk versi 64-bit saja
```sh
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\RunOnce
```
