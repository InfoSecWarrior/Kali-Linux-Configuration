# Introduction to Kali Linux

Kali Linux is a Debian-based Linux distribution.
Kali Linux is to be used by those who are professional penetration testers, cybersecurity experts, ethical hackers, or those who know how to operate it.
When it comes to penetration testing, hacking and offensive Linux distributions, one of the first thing to be mentioned is Kali Linux.

# These are steps to configure Kali Linux for Penetration Testing or Bug Bounty.

It ranges from the setup the repository to the installation of NVIDIA graphic drivers in Optimus laptops and for setting up the PYTHON(latest/2.7) and PIP(latest/2.7). 

1. Set the password for root:
		
		sudo passwd

2. Switch to root: 
		
		sudo su

3. APT transport for downloading via the HTTP Secure protocol (HTTPS)

		apt install apt-transport-https

4. Google search: Kali repo mirror (htpps://http.kali.org/README.mirrorlist) and copy the mirror repo of any region.
		
		vim /etc/apt/source.list 

5. Replace the current repo's url with the mirror repo's url. (It will help speed up the downloading processes) But if it doesn't work and the downloading speed is slow, undo the changes in the file.
		
		apt update
		
		apt upgrade 
		
		apt install kali-linux-large
		
		apt dist-upgrade
		
		apt full-upgrade

6. Restart Required by using the command: 
		
		[ -f /var/run/reboot-required ] && reboot -f
 

6. Install kernel headers

		apt install linux-headers-(uname -r)

7. Restart by using the command: 
		
		[ -f /var/run/reboot-required ] && reboot -f

    # Installing NVIDIA DRIVERS

8. Install NVIDIA drivers in Optimus laptop:

		uname -a
  
9. Verify you have hybrid graphics 
  
		lspci | grep -E "VGA|3D"
    
10. Disable nouveau
  
		echo -e " blacklist nouveau\nooptions nouveau modeset=0\nalias nouveau off" > /etc/modprobe.dblacklist-nouveau.conf

11. Generate an initramfs image
		 
		update-initramfs -u && reboot

12. System will reboot and nouveau should be disabled. Verify if nouveau is disabled:
   
		lsmod | grep -i nouveau
		
    If it shows nothing, it means nouveau is disabled.
    
 
13. When the nouveau is disabled Install NVIDIA driver from Kali Linux's repo:
  
		apt install nvidia-xconfig nvidia-driver
    
14. If the command shows error just give the following command(Optional Command)
     
		apt install nvidia-xconfig
     

    After the successful installation of NVIDIA driver, install "Cuda" from the official NVIDIA website (Cuda will start encoding in Kali Linux and it will     help run a few programs better).      


    > If the drivers are not working properly or if it doesn't support dual monitor, you'll have to follow the following steps:

15. We have to find BusID of our NVIDIA card:
  
		nvidia-xconfig --query-gpu-info | grep 'BusID :' | cut -d ' ' -f6
     
    It will show the BusID like “ PCI:1:0:0 ”
    
    
16. Open the file in Terminal
  
		vim /etc/X11/xorg.conf

    - Copy This Code       


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

17. Now we have to create some scripts according to our display manager 
  
		vim /usr/share/gdm/greeter/autostart/optimus.desktop
    
    - Copy This Code

     
					[Desktop Entry]
					Type=Application
					Name=Optimus
					Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
					NoDisplay=true
					X-GNOME-Autostart-Phase=DisplayServer


  

18. Copy this file to designated location 
  
		cp -v /usr/share/gdm/greeter/autostart/optimus.desktop /etc/xdg/autostart/optimus.desktop
    
    
19. Restart by using the command:
  
		[ -f /var/run/reboot-required ] && reboot -f 
      
      
      



    Caution⚠

    In case of the mis-configuration or any error, if you want to remove the NVIDIA drivers just follow the commands mentioned below and the drivers will       be removed:

		apt-get remove --purge nvidia*
			   
		rm -rf /etc/X11/xorg.conf
			   
		dpkg --configure -a
			  
		apt autoremove 



    After the successful installation of NVIDIA drivers and everything, don't run the following commands as it can result in the deletion if the drivers.

		apt update
 		 
		apt upgrade
 		 
		apt autoremove
 		 
		apt autoclean




20. For checking if the NVIDIA driver is working or not
		 
		hashcat -I 
 
21. Encoding enabled or not:
		 
		ffmpg -encoders 2>/dev/null | grep nvenc


22. Benchmark testing:
		 
		hashcat -b



    After the basic setup of Kali Linux you should check for the versions of python and pip installed in Kali. If their version is mis-matched, they will       conflict and won't work properly. After the basic setup of Kali Linux you should check for the versions of python and pip installed in Kali. If their       version is mis-matched, they will conflict and won't work properly.

23. To check the latest version of python use the command:

		python -V
   
24. Also check for the latest version of pip using the command:

		pip -V 
      
    We will install the 2.7 version of both Python and Pip:   

    Check for those versions if they are installed or not ny using the following commands:

25. For Pyhon
       
		python2.7 -V

26. For pip

		pip2.7 -V 
      
      
    If Pip version 2.7 is not installed, google “https://bootstrap.pypa.io/pip/” and from there download the 2.7 version.
  
27.  Run the command:

		 python2.7 get-pip.py
    

28. You can install Sublime editor from the official Sublime website.
		
		wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

		echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

		apt update (If You install CUDA Toolkit So Comment it's Repositories)

		apt install sublime-text

29. After the successful installation of the above you can just install the basic tools that we need by the single command:


		apt install rclone vim fonts-lato fonts-open-sans fonts-roboto fonts-mononoki fonts-indic grc python-is-python3 gcc-multilib g++-multilib libtesseract-dev jq python3-pip openvpn network-manager-openvpn network-manager-openvpn-gnome krita testssl.sh dirsearch wkhtmltopdf virtualbox virtualbox-ext-pack virtualbox-guest-additions-iso golang remmina remmina-plugin-rdp remmina-plugin-secret youtube-dl
      
 
30. To check the system is encoding or not, give the following command:

		ffmpeg -encoders 2>/dev/null | grep nvenc
      
      
      
Caution⚠      

After the setup is complete, don't update or upgrade the version of Kali Linux until it is extremely necessary because if you do there might be chances of mis-configuration or versions mis-matching and Kali will not work properly after that.

After all this, the Basic SetUp for Kali Linux is complete and you're good to go!!
