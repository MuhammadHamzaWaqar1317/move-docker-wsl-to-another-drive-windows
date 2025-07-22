# move-docker-wsl-to-another-drive-windows

# 🐳 Move Docker WSL 2 Data to Another Drive (Windows)

Free up space on your `C:` drive by relocating Docker’s **WSL 2 virtual disk (`ext4.vhdx`)** to another drive like `G:` using symbolic links. This guide walks you through the complete process safely and cleanly.

> ⚠️ **Warning:** This will delete all existing Docker containers, images, and volumes. Make sure to back up or export anything important before proceeding.

> 📝 **Note:** This method is for **Docker using the WSL 2 backend** on Windows.

---

## 📁 Docker Data Location Breakdown

Docker with WSL 2 stores data here:
C:\Users<YourUsername>\AppData\Local\Docker\wsl\disk\ext4.vhdx ← Linux containers/images/volumes
C:\Users<YourUsername>\AppData\Local\Docker\wsl\main ← Metadata and supporting files

In this tutorial, we will move:
C:\Users\Dell\AppData\Local\Docker\wsl\disk\ext4.vhdx → G:\DockerData\ext4.vhdx


---

## 🛠️ Step-by-Step Guide

### ✅ Step 1: Shut Down Docker and WSL

Open PowerShell as **Administrator** and run:

```powershell
wsl --shutdown
```
### ✅ Step 2: Create Target Folder on Another Drive
mkdir G:\DockerData

### ✅ Step 3: Move Docker’s ext4.vhdx File
move "C:\Users\Dell\AppData\Local\Docker\wsl\disk\ext4.vhdx" "G:\DockerData\ext4.vhdx"

### ✅ Step 4: Create a Symbolic Link
New-Item -ItemType SymbolicLink -Path "C:\Users\Dell\AppData\Local\Docker\wsl\disk\ext4.vhdx" -Target "G:\DockerData\ext4.vhdx"

### ✅ Step 5: Set Permissions on New Location
Go to G:\DockerData in File Explorer

Right-click the folder → Properties → Security tab

Click Edit and grant Full Control to:

SYSTEM

Administrators

Your current Windows user

Click Apply and OK

✅ Run Docker Desktop as Administrator from now on.

### ✅ Step 6: Restart Docker and Verify
Start Docker Desktop again and check everything is working:
docker info

### After these steps your folder structure will look like this
G:\DockerData
└── ext4.vhdx ← moved here
C:\Users\Dell\AppData\Local\Docker\wsl\disk
└── ext4.vhdx → symbolic link to G:\DockerData\ext4.vhdx

