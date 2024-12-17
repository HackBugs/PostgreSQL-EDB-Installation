> ## Apt, DNF, aur YUM sab Linux-based package managers hain, jo alag-alag Linux distributions me use hote hain. In sab ka kaam package management hai, lekin har ek ka use specific Linux distributions ke liye hota hai. Yahan ek list di gayi hai jisme bataya gaya hai ki kaunsa package manager kis distribution me use hota hai:

### 1. **APT (Advanced Package Tool)**
   - **Use in**: Debian-based distributions
   - **Examples of Distributions**:
     - **Ubuntu**
     - **Debian**
     - **Linux Mint**
     - **Pop!_OS**
     - **Kali Linux**
   - **Commands**:
     - Install package: `sudo apt install <package_name>`
     - Update package list: `sudo apt update`
     - Upgrade packages: `sudo apt upgrade`
     - Remove package: `sudo apt remove <package_name>`
     - Search package: `apt search <package_name>`

### 2. **DNF (Dandified YUM)**
   - **Use in**: Red Hat-based distributions (modern versions)
   - **Examples of Distributions**:
     - **Fedora**
     - **RHEL (Red Hat Enterprise Linux)** (from version 8 and above)
     - **CentOS** (from version 8 and above)
     - **Rocky Linux**
     - **AlmaLinux**
   - **Commands**:
     - Install package: `sudo dnf install <package_name>`
     - Update package list: `sudo dnf check-update`
     - Upgrade packages: `sudo dnf upgrade`
     - Remove package: `sudo dnf remove <package_name>`
     - Search package: `dnf search <package_name>`

### 3. **YUM (Yellowdog Updater, Modified)**
   - **Use in**: Older Red Hat-based distributions
   - **Examples of Distributions**:
     - **CentOS** (7 and earlier)
     - **RHEL (Red Hat Enterprise Linux)** (7 and earlier)
     - **Scientific Linux**
     - **Oracle Linux** (older versions)
   - **Commands**:
     - Install package: `sudo yum install <package_name>`
     - Update package list: `sudo yum check-update`
     - Upgrade packages: `sudo yum update`
     - Remove package: `sudo yum remove <package_name>`
     - Search package: `yum search <package_name>`

### Summary of Usage:

- **APT** is used in **Debian-based** systems (e.g., Ubuntu, Debian).
- **DNF** is used in **newer Red Hat-based** systems (e.g., Fedora, RHEL 8+, CentOS 8+).
- **YUM** is used in **older Red Hat-based** systems (e.g., CentOS 7, RHEL 7).

### Key Differences:
- **APT** is the default for Debian/Ubuntu systems.
- **DNF** is the modern replacement for YUM in Fedora and RHEL-based distributions.
- **YUM** was used before DNF in older Red Hat-based systems.

<hr>

> ## Haan, apt, dnf, aur yum ke alawa bhi kuch aur package managers hain jo alag-alag Linux distributions aur unke versions mein use kiye jaate hain. Main unka bhi mention kar raha hoon. Yahan ek aur detailed list hai, jisme kuch aur popular package managers ko include kiya gaya hai:

### 1. **APT (Advanced Package Tool)**
   - **Use in**: Debian-based distributions
   - **Examples**: Ubuntu, Debian, Linux Mint, Pop!_OS, Kali Linux
   - **Commands**: 
     - `sudo apt install <package_name>`
     - `sudo apt update`
     - `sudo apt upgrade`

### 2. **DNF (Dandified YUM)**
   - **Use in**: Red Hat-based (modern versions)
   - **Examples**: Fedora, RHEL 8+, CentOS 8+, AlmaLinux, Rocky Linux
   - **Commands**:
     - `sudo dnf install <package_name>`
     - `sudo dnf check-update`
     - `sudo dnf upgrade`

### 3. **YUM (Yellowdog Updater, Modified)**
   - **Use in**: Older Red Hat-based distributions
   - **Examples**: CentOS 7, RHEL 7, Scientific Linux
   - **Commands**:
     - `sudo yum install <package_name>`
     - `sudo yum update`
     - `sudo yum remove <package_name>`

---

### 4. **Zypper**
   - **Use in**: SUSE-based distributions
   - **Examples**: openSUSE, SUSE Linux Enterprise
   - **Commands**:
     - `sudo zypper install <package_name>`
     - `sudo zypper update`
     - `sudo zypper remove <package_name>`
     - `sudo zypper search <package_name>`

### 5. **Pacman**
   - **Use in**: Arch Linux and its derivatives
   - **Examples**: Arch Linux, Manjaro, EndeavourOS
   - **Commands**:
     - `sudo pacman -S <package_name>`
     - `sudo pacman -Sy` (update package database)
     - `sudo pacman -R <package_name>`
     - `sudo pacman -Syu` (full system upgrade)

### 6. **Snap**
   - **Use in**: Cross-distribution package manager (supports many Linux distros)
   - **Examples**: Ubuntu, Fedora, Debian, Arch, etc.
   - **Commands**:
     - `sudo snap install <package_name>`
     - `sudo snap remove <package_name>`
     - `sudo snap list`

### 7. **Flatpak**
   - **Use in**: Cross-distribution package manager (like Snap)
   - **Examples**: Fedora, Ubuntu, Debian, Arch, etc.
   - **Commands**:
     - `flatpak install <package_name>`
     - `flatpak update`
     - `flatpak uninstall <package_name>`
     - `flatpak list`

### 8. **Portage**
   - **Use in**: Gentoo and Gentoo-based distributions
   - **Examples**: Gentoo Linux, Sabayon
   - **Commands**:
     - `sudo emerge <package_name>`
     - `sudo emerge --update <package_name>`
     - `sudo emerge --unmerge <package_name>`

### 9. **Pkg**
   - **Use in**: FreeBSD (also used in other BSD variants)
   - **Examples**: FreeBSD, TrueOS
   - **Commands**:
     - `sudo pkg install <package_name>`
     - `sudo pkg update`
     - `sudo pkg delete <package_name>`

### 10. **Nix**
   - **Use in**: NixOS and Nix package manager
   - **Examples**: NixOS
   - **Commands**:
     - `nix-env -iA nixpkgs.<package_name>`
     - `nix-env -u`
     - `nix-env -e <package_name>`

---

### Summary of Popular Package Managers:

| **Package Manager** | **Used in** | **Examples** | **Commands** |
|---------------------|-------------|--------------|--------------|
| **APT**             | Debian-based | Ubuntu, Debian, Linux Mint, Kali Linux | `apt install <pkg>`, `apt update`, `apt upgrade` |
| **DNF**             | Red Hat-based (modern) | Fedora, RHEL 8+, CentOS 8+, AlmaLinux | `dnf install <pkg>`, `dnf upgrade` |
| **YUM**             | Red Hat-based (old) | CentOS 7, RHEL 7, Scientific Linux | `yum install <pkg>`, `yum update` |
| **Zypper**          | SUSE-based | openSUSE, SUSE Linux Enterprise | `zypper install <pkg>`, `zypper update` |
| **Pacman**          | Arch-based | Arch Linux, Manjaro, EndeavourOS | `pacman -S <pkg>`, `pacman -Syu` |
| **Snap**            | Cross-distribution | Ubuntu, Fedora, Debian, Arch | `snap install <pkg>`, `snap list` |
| **Flatpak**         | Cross-distribution | Fedora, Ubuntu, Arch, Debian | `flatpak install <pkg>`, `flatpak update` |
| **Portage**         | Gentoo-based | Gentoo, Sabayon | `emerge <pkg>`, `emerge --update` |
| **Pkg**             | BSD-based | FreeBSD, TrueOS | `pkg install <pkg>`, `pkg update` |
| **Nix**             | NixOS-based | NixOS | `nix-env -iA <pkg>`, `nix-env -u` |

---

### Key Points:
- **Snap** aur **Flatpak** cross-distribution package managers hain jo multiple Linux distributions pe kaam karte hain.
- **Portage** aur **Nix** jaise package managers distribution-specific hain, lekin unki apni unique features hoti hain.
- **Zypper** aur **Pacman** zyada tar SUSE-based aur Arch-based systems me use kiye jaate hain.

<hr>
