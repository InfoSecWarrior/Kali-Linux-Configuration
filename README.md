# Introduction to Kali Linux.

Kali Linux is a Debian-based Linux distribution.
Kali Linux is to be used by those who are professional penetration testers, cybersecurity experts, ethical hackers, or those who know how to operate it.
When it comes to penetration testing, hacking and offensive Linux distributions, one of the first thing to be mentioned is Kali Linux.

# These are steps to configure Kali Linux for Penetration Testing or Bug Bounty.

It ranges from the setup the repository to the installation of NVIDIA graphic drivers in Optimus laptops and for setting up the PYTHON(latest/2.7) and PIP(latest/2.7). 

1. **Set the password for root:**
	```	bash
	sudo passwd
	```
2. **Switch to root:** 
	```bash
	sudo su
	```
3. **APT transport for downloading via the HTTP Secure protocol (HTTPS)**
	
	```bash
	apt install apt-transport-https
	```
	
4. **Google search: Kali repo mirror (htpps://http.kali.org/README.mirrorlist) and copy the mirror repo of any region.**
	```bash
	vim /etc/apt/sources.list 
    ```

5. **Replace the current repo's url with the mirror repo's url. (It will help speed up the downloading processes) But if it doesn't work and the downloading speed is slow, undo the changes in the file.**
		
    ```bash
	apt update
	
	apt upgrade 
		
	apt install kali-linux-large
		
	apt dist-upgrade
		
	apt full-upgrade
    ```

6. **Restart Required by using the command:**

    ```bash
    [ -f /var/run/reboot-required ] && reboot -f
    ```

7. **Install kernel headers.**

    ```bash
	apt install linux-headers-$(uname -r)
    ```

8. **Restart by using the command:**

    ```bash
	[ -f /var/run/reboot-required ] && reboot -f
    ```

    # Installing NVIDIA DRIVERS.

9. **Install NVIDIA drivers in Optimus laptop:**

    ```bash
	uname -a
    ```

10. **Verify you have hybrid graphics.**
  
    ```bash
	lspci | grep -E "VGA|3D"
    ```

11. **Disable nouveau**

    ```bash
	echo -e "blacklist nouveau\noptions nouveau modeset=0\nalias nouveau off" > /etc/modprobe.d/blacklist-nouveau.conf
    ```

12. **Generate an initramfs image**
	
    ```bash
	update-initramfs -u && reboot
    ```

13. **System will reboot and nouveau should be disabled. Verify if nouveau is disabled:**
    
    ```bash
	lsmod | grep -i nouveau
	```	
    If it shows nothing, it means nouveau is disabled.

14. **When the nouveau is disabled Install NVIDIA driver from Kali Linux's repo:**

    ```bash
	apt install nvidia-xconfig nvidia-driver
    ```

15. **If the command shows error just give the following command(Optional Command)**

    ```bash
	apt install nvidia-xconfig
    ``` 
    After the successful installation of NVIDIA driver, install "Cuda" from the official NVIDIA website (Cuda will start encoding in Kali Linux and it will     help run a few programs better).      


    > If the drivers are not working properly or if it doesn't support dual monitor, you'll have to follow the following steps:

16. **We have to find BusID of our NVIDIA card:**
    
    ```bash
	nvidia-xconfig --query-gpu-info | grep 'BusID :' | cut -d ' ' -f6
    ```
    
    It will show the BusID like " PCI:1:0:0 "
    
17. **Open the file in Terminal**
    
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

18. **Now we have to create some scripts according to our display manager**
    
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
 

19. **Copy this file to designated location**
    
    ```bash
	cp -v /usr/share/gdm/greeter/autostart/optimus.desktop /etc/xdg/autostart/optimus.desktop
    ```

19. **Restart by using the command:**
    
    ```bash
	[ -f /var/run/reboot-required ] && reboot -f 
    ```  

20. **For checking if the NVIDIA driver is working or not**
	
    ```bash
	hashcat -I 
    ```

21. **Encoding enabled or not:**
	
    ```bash
	apt install ffmpeg
	ffmpeg -encoders 2>/dev/null | grep nvenc
    ```

22. **Benchmark testing:**
	
    ```bash
	hashcat -b
    ```


    After the basic setup of Kali Linux you should check for the versions of python and pip installed in Kali. If their version is mis-matched, they will       conflict and won't work properly. After the basic setup of Kali Linux you should check for the versions of python and pip installed in Kali. If their       version is mis-matched, they will conflict and won't work properly.

23. **To check the latest version of python use the command:**

    ```bash
	python -V
    ```

24. **Also check for the latest version of pip using the command:**

    ```bash
	pip -V 
    ```  
    We will install the 2.7 version of both Python and Pip:   

    Check for those versions if they are installed or not ny using the following commands:

25. **For Pyhon**

    ```bash
	python2.7 -V
    ```

26. **For pip**

    ```bash
	pip2.7 -V 
    ```  
      
    If Pip version 2.7 is not installed, google “https://bootstrap.pypa.io/pip/” and from there download the 2.7 version.
  
27. **Run the comman**

    ```bash
    python2.7 get-pip.py
    ```

28. **You can install Sublime editor from the official Sublime website.**
    
    ```bash
	wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

	echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

	apt update (If You install CUDA Toolkit So Comment it's Repositories)

	apt install sublime-text
    ```

29. **After the successful installation of the above you can just install the basic tools that we need by the single command:**

    ```bash
	apt install build-essential cmake ninja-build libgtkmm-3.0-dev libgtksourceviewmm-3.0-dev libxml++2.6-dev libsqlite3-dev gettext libgspell-1-dev libcurl4-openssl-dev libuchardet-dev libfribidi-dev libvte-2.91-dev libfmt-dev libspdlog-dev rclone vim fonts-lato fonts-open-sans fonts-roboto fonts-mononoki fonts-indic grc python3 python-is-python3 gcc-multilib g++-multilib libtesseract-dev jq python3-pip openvpn network-manager-openvpn network-manager-openvpn-gnome  testssl.sh dirsearch wkhtmltopdf virtualbox virtualbox-ext-pack virtualbox-guest-additions-iso golang remmina remmina-plugin-rdp remmina-plugin-secret youtube-dl flameshot ruby whois git curl libpcap-dev wget zip python3-dev pv dnsutils libssl-dev libffi-dev libxml2-dev libxslt1-dev zlib1g-dev nmap apt-transport-https lynx tor medusa xvfb libxml2-utils procps bsdmainutils libdata-hexdump-perl wget curl tmux git nmap masscan unzip chromium rsync coreutils net-tools htop prips xmlstarlet gnome-power-manager jython mesa-utils wmctrl
    ```
      
### Caution⚠   

-   **In case of the mis-configuration or any error, if you want to remove the NVIDIA drivers just follow the commands mentioned below and the drivers will be removed:**

    ```bash
    apt-get remove --purge nvidia*

    rm -rf /etc/X11/xorg.conf
        
    dpkg --configure -a
        
    apt autoremove 
    ```


-   **After the successful installation of NVIDIA drivers and everything, don't run the following commands as it can result in the deletion if the drivers.**

    ```bash
	apt update
 	
	apt upgrade
 	
	apt autoremove
 	
	apt autoclean
    ```

*After the setup is complete, don't update or upgrade the version of Kali Linux until it is extremely necessary because if you do there might be chances of mis-configuration or versions mis-matching and Kali will not work properly after that.*



## Multi gesture  in Kali Linux.  

<B>`To get multi-gesture functionality on the trackpad in Kali Linux, you need touchegg tool.`
  </B>
  
 You don't get the tool in repo so you need the download the `.deb` package via its **[github releases](https://github.com/JoseExposito/touchegg/releases)**

 Once you have **downloaded** the package install it. (there are **2 ways to install** the *package*)

-  By **apt.**

    ```bash
    wget https://github.com/JoseExposito/touchegg/releases/download/2.0.16/touchegg_2.0.16_amd64.deb
    ```
    ```bash
    apt install ./touchegg_2.0.16_amd64.deb
    ```

 
-   By **dpkg.**

     ```bash
    wget https://github.com/JoseExposito/touchegg/releases/download/2.0.16/touchegg_2.0.16_amd64.deb
    ```
    ```bash
    dpkg --install ./touchegg_2.0.16_amd64.deb
    ```
    ```bash
    sudo apt --fix-broken install
    ```

-   **Check the status of the service and start it.**

    ```bash
    systemctl status touchegg.service
    ```

    ```bash
    systemctl start touchegg.service
    ```

-   **Enable the service**

    ```bash
    systemctl enable touchegg.service
    ```

-   **Restart by using the command**

    ```bash
	[ -f /var/run/reboot-required ] && reboot -f
    ```

**For the tool to work you need [GNOME shell integration](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep) on your browser.**

Add the extention to your browser and open the extenstion search for [X11](https://extensions.gnome.org/extension/4033/x11-gestures/) `toggle it to ON`

**it will prompt you to install the extention approve it**

*Restart your laptop, after restart try 3 fingure gesture to zoom out of the screen.*

Can also customize the gestures accordingly in my opinion default just work fine.




## Right click not working in trackpad.

When you install Kali Linux sometimes you come across a weird issue regarding the trackpad
RIGHT click doesn't work as intended i.e. It does not show you the dialog box of copy,paste etc (the one by which you used to refresh your windows PC with)

-   **To bring it back:**

    ```bash
    apt update
    ```
    ```bash
    apt install xserver-xorg-input-synaptics
    ```

    ```bash
    synclient tapbutton1=1
    ```

-   **Restart by using the command**

    ```bash
	[ -f /var/run/reboot-required ] && reboot -f
    ```

**After all this, the Basic SetUp for Kali Linux is complete and you're good to go!!**
