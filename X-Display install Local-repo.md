> # Aapko jo error mil raha hai (java.awt.HeadlessException) wo isliye hai kyunki aap terminal/command line se GUI (Graphical User Interface) application ko run karne ki koshish kar rahe hain, aur aapke system me GUI support nahi hai (headless mode me chal raha hai). JBoss installer ko GUI ki zarurat hoti hai, jo terminal ke through available nahi hota.

### Solution:
Aapko **headless mode** me installer ko run karna hoga, ya phir GUI ko disable karte hue installer ko run karna hoga.

#### Headless mode me installer run karne ke liye:

1. **Install in silent mode**:
   JBoss installer ko silent mode me run karne ke liye, aapko `-silent` option ka use karna hoga. Yeh mode GUI ke bina installation ko complete karega.

   Command:
   ```bash
   java -jar jboss-eap-8.0.0-installer.jar -silent
   ```

2. **Check installation log**:
   Silent mode me installation complete hone ke baad, aap log files check kar sakte hain installation ki details ke liye.

   Log file location: `/tmp/jboss-eap-8.0.0-installer-*.log`

---

### Alternative solution (if GUI is needed):
Agar aapko GUI ki zarurat hai aur aap graphical interface wale system pe kaam kar rahe hain, to aapko `X11` ka setup karna hoga.

1. **Install X11 server (if not installed)**:
   - RHEL 8 me `X11` ko install karne ke liye, command:
     ```bash
     sudo dnf install xorg-x11-server-Xorg
     ```
   
2. **Set DISPLAY variable**:
   Agar aap remote server se kaam kar rahe hain to aapko X11 forwarding enable karna hoga.
   
   Agar local machine pe kaam kar rahe hain to ensure `DISPLAY` variable set ho.

   Command:
   ```bash
   export DISPLAY=:0
   ```

3. **Run the installer again**:
   ```bash
   java -jar jboss-eap-8.0.0-installer.jar
   ```

<hr>

> # How to Install GUI Package & Switch From CLI to GUI in Redhat Linux 8 Server [Link](https://www.redhat.com/en/blog/configure-systemd-startup-targets)

```
yum repo list
yum install 'dnf-command(repo)'
yum groupinstall "Server with GUI"
sudo dnf groupinstall "Server with GUI"

systemctl list-units --type=target
sudo systemctl set-default graphical.target
init 6 // reboot
runlevel
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target
init 6 // reboot
```

> # Make Local repo and configuare

Yahan par ek simple bash script diya gaya hai jo aapko local repository banane aur `.rpm` files ko install karne mein madad karega.

### Bash Script: `create_local_repo.sh`

```bash
cd /etc/yum.repos.d/
touch local.repo
vim local.repo

[local-repo]
name=Local Repository
baseurl=file:///opt/edb2/el_8_x86_64
enabled=1
gpgcheck=0

sudo dnf clean all

sudo dnf install package-name
rpm -q package-name
```

<hr>

> # REHL Minimal Install Without GUI
