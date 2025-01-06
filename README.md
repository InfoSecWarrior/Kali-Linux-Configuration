# Introduction to Kali Linux.

`Kali Linux is a Debian-based Linux distribution.
Kali Linux is to be used by those who are professional penetration testers, cybersecurity experts, ethical hackers, or those who know how to operate it.
When it comes to penetration testing, hacking and offensive Linux distributions, one of the first thing to be mentioned is Kali Linux.`

# These are steps to configure Kali Linux for Penetration Testing or Bug Bounty.

<B>`It ranges from changing/editing the repository for the installation of NVIDIA graphic drivers in Optimus laptops to setting up PYTHON(latest/2.7) and PIP(latest/2.7). 
` </B>

**Set the password for root**
	
```bash
sudo passwd
```
 **Switch to root:** 
	
```bash
sudo su
```
  
  **APT transport for downloading via the HTTP Secure protocol (HTTPS)**
	
```bash
apt install apt-transport-https
```
	
 **Google search: Kali [repo mirror](https://http.kali.org/README.mirrorlist) and copy the mirror repo of any region in:**
	
```bash
vim /etc/apt/sources.list 
```
THE **default** REPO
```bash
 deb http://http.kali.org/kali kali-rolling main contrib non-free
```
**Fast** REPO

```bash
 deb https://kali.download/kali kali-rolling main contrib non-free
```
**Replace the current/default repo's url with the mirror repo's url. (It will help speed up the downloading processes) 
But if it doesn't work and the downloading speed is slow, undo the changes in the file.**
		
```bash
apt update
	
apt upgrade 

apt install kali-linux-everything

apt install kali-linux-large
		
apt dist-upgrade

apt full-upgrade
 ```

 **Restart Required by using the command:**

```bash
 [ -f /var/run/reboot-required ] && reboot
```

**Install kernel headers.**

```bash
apt install linux-headers-$(uname -r)
```

**Restart by using the command:**

```bash
[ -f /var/run/reboot-required ] && reboot
```

**note:** The above commands will install Kali Linux on PC/Laptop with integrated graphics, you can skip the NVIDIA part and directly go to [python installation](https://github.com/InfoSecWarrior/Kali-Linux-Configuration#python)


   # Installing NVIDIA DRIVERS.

**Install NVIDIA drivers in Optimus laptop:**

```bash
uname -a
```

 **Verify you have hybrid graphics.**
  
```bash
lspci | grep -E "VGA|3D"
```

 **Disable nouveau**

```bash
echo -e "blacklist nouveau\noptions nouveau modeset=0\nalias nouveau off" > /etc/modprobe.d/blacklist-nouveau.conf
```

**Generate an initramfs image**
	
```bash
update-initramfs -u && reboot
```

 **System will reboot and nouveau should be disabled. Verify if nouveau is disabled:**
    
```bash
lsmod | grep -i nouveau
```	
   
   <B>`If it shows nothing, it means nouveau is disabled.`</B>

**When the nouveau is disabled Install NVIDIA driver from Kali Linux's repo:**

```bash
apt install nvidia-xconfig nvidia-driver
```

**If the command shows error just give the following command(Optional Command)**

```bash
apt install nvidia-xconfig
```

**After the successful installation of NVIDIA driver, install [**'Cuda'**](https://developer.nvidia.com/cuda-downloads?target_os=Linux) from the official NVIDIA website**

`(Cuda will start encoding in Kali Linux and it will help run a few programs better).`      


*If the drivers are not working properly or if it doesn't support dual monitor, you'll have to follow the following steps:*

**We have to find BusID of our NVIDIA card:**
    
```bash
nvidia-xconfig --query-gpu-info | grep 'BusID :' | cut -d ' ' -f6
```
    
It will show the **BusID** like **" PCI:1:0:0 "**
    
 **Open the file in Terminal**
    
```bash
vim /etc/X11/xorg.conf
```

   - **Copy This Code**
	
```bash
Section "ServerLayout"
    Identifier "layout"
    Screen 0 "nvidia"
    Inactive "intel"
EndSection

Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "PCI:1:0:0"
EndSection

Section "Screen"
    Identifier "nvidia"
    Device "nvidia"
    Option "AllowEmptyInitialConfiguration"
EndSection

Section "Device"
    Identifier "intel"
    Driver "modesetting"
EndSection

Section "Screen"
    Identifier "intel"
    Device "intel"
EndSection
```

**Now we have to create some scripts according to our display manager**
    
```bash
vim /usr/share/gdm/greeter/autostart/optimus.desktop
```
   - **Copy This Code**

 ```bash
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer
```
 

**Copy this file to designated location**
    
```bash
cp -v /usr/share/gdm/greeter/autostart/optimus.desktop /etc/xdg/autostart/optimus.desktop
```

**Restart by using the command:**
    
```bash
[ -f /var/run/reboot-required ] && reboot -f 
```  

**For checking if the NVIDIA driver is working or not**
	
```bash
hashcat -I 
```

**Encoding enabled or not:**
	
```bash
apt install ffmpeg
ffmpeg -encoders 2>/dev/null | grep nvenc
```

**Benchmark testing:**
	
```bash
hashcat -b
```
## PYTHON

After the basic setup of Kali Linux you should check for the ***versions*** of **python** and **pip installed in Kali**. If the version is mis-matched, they will       conflict and won't work properly.

**To check the latest version of python use the command:**

```bash
python -V
```

**Also check for the latest version of pip using the command:**

```bash
pip -V 
```  
We will install the **2.7 version of both Python and Pip**:   

Check for those versions if they are installed or not ny using the following commands:

**For Python**

```bash
python2.7 -V
```

**For pip 2.7**

```bash
pip2.7 -V 
```  
Download pip2.7 version.
    
```bash
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
```
Install your pip2.7 version package.
```bash
python2.7 get-pip.py
```

**For pip**

```bash
pip -V 
```  
Check And Download the latest pip version package https://bootstrap.pypa.io/pip/
    
Install your latest version package.
```bash
python get-pip.py
```

## Install and Configure ZSH Shell

Install ZSH:

```bash
apt install zsh
```

Download and use the `.zshrc` configuration file:

```bash
cd
wget https://raw.githubusercontent.com/InfoSecWarrior/Kali-Linux-Configuration/refs/heads/main/.zshrc
```

Set ZSH as the default shell:
```bash
chsh -s $(which zsh)
```
Restart the terminal to apply changes.

    
**You can install Sublime editor from the official Sublime website.**

Caution ⚠ (If You install CUDA Toolkit So Comment it's Repositories) 

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null

echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

apt update 

apt install sublime-text
```

**After the successful installation of the above you can just install the basic tools that we need by the single command:**

```bash
apt install build-essential cmake ninja-build libgtkmm-3.0-dev libgtksourceviewmm-3.0-dev libxml++2.6-dev libsqlite3-dev gettext libgspell-1-dev libcurl4-openssl-dev libuchardet-dev libfribidi-dev libvte-2.91-dev libfmt-dev libspdlog-dev rclone vim fonts-lato fonts-open-sans fonts-roboto fonts-mononoki fonts-indic grc python3 python-is-python3 gcc-multilib g++-multilib libtesseract-dev jq python3-pip openvpn network-manager-openvpn network-manager-openvpn-gnome  testssl.sh dirsearch wkhtmltopdf virtualbox virtualbox-ext-pack virtualbox-guest-additions-iso golang remmina remmina-plugin-rdp remmina-plugin-secret flameshot ruby whois git curl libpcap-dev wget zip python3-dev pv dnsutils libssl-dev libffi-dev libxml2-dev libxslt1-dev zlib1g-dev nmap apt-transport-https lynx tor medusa xvfb libxml2-utils procps bsdmainutils libdata-hexdump-perl wget curl tmux git nmap masscan unzip chromium rsync coreutils net-tools htop prips xmlstarlet gnome-power-manager jython mesa-utils wmctrl libgl1-mesa-dev xorg-dev
```
      
### Caution⚠   

-   **In case of the mis-configuration or any error, if you want to remove the NVIDIA drivers just follow the commands mentioned below and the drivers will be removed:**

```bash
apt-get remove --purge nvidia*

rm -rf /etc/X11/xorg.conf
        
dpkg --configure -a
        
apt autoremove 
```


**After the successful installation of NVIDIA drivers and everything,** 

**DON't run the following commands as it can result in the deletion if the drivers.**

```bash
apt autoremove
 	
apt autoclean
```

**After the setup is complete, don't update or upgrade the version of Kali Linux until it is extremely necessary** 

because if you do there might be chances of **mis-configuration or versions mis-matching** and Kali will not work properly after that.*




**After all this, the Basic SetUp for Kali Linux is complete and you're good to go!!**


**restart you laptop**



throughout the code wherever you see text in [this fomat]() it contains important URL links for respective tools or commands
OPEN the links with **'ctl key pressed while you click'** so that it will **open in new tab**.
