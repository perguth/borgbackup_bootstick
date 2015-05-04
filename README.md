# attic_bootstick

- [x] Configure proxy:  
  `echo "export http_proxy="http://10.10.10.9:3128" >> ~/.bashrc && source ~/.bashrc`
- [ ] Fix Ubuntus privacy violations:  
```sh
git clone https://gist.github.com/5c99244436d24cfa4123.git  
chmod +x 5c99244436d24cfa4123/fixubuntu.sh  
./5c99244436d24cfa4123/fixubuntu.sh
rm -rf 5c99244436d24cfa4123
```
- [ ] Add Universe sources:  
  `apt-add-repository -y 'deb http://archive.ubuntu.com/ubuntu/ trusty universe'`
- [x] Update Ubuntu:  
  `sudo apt-get update && sudo apt-get upgrade -y`
- [x] Install Customizer requirements:  
  `sudo apt-get install make binutils g++ python2.7 python2.7-dev python-qt4 pyqt4-dev-tools squashfs-tools xorriso x11-xserver-utils xserver-xephyr qemu-kvm qt4-linguist-tools -y`
- [ ] Install Customizer:  
```sh
cd ~
git clone https://github.com/clearkimura/Customizer.git
cd Customizer
make && sudo make install
cd .. && rm -rf Customizer
```
- [ ] Chroot into Customizer:  
  `sudo customizer-gui`

-> set up proxy
- [ ] Fix dhcp:  
  `nano /etc/resolv.conf` insert own nameserver IP
- [ ] Remove unnecessary software:  
  `apt-get autoremove libreoffice* -y`
- [x] Install bootstick package selection:  
```sh
apt-get install mc rsync git python3.4 python3.4-dev openssh-server python-virtualenv openssl libssl-dev python3-llfuse fuse libacl1 libacl1-dev attr python-tox vim indicator-multiload -y
apt-get install mdadm smartmontools --no-install-recommends -y
apt-get autoremove -y && apt-get clean
```
- [ ] Autostart `indicator-multiload`:  
  <sup>[[src]](http://askubuntu.com/a/348107/207593)</sup>
  <sup>[[src]](http://askubuntu.com/questions/48321/how-do-i-start-applications-automatically-on-login)</sup>
  `nano /etc/guest-session/prefs.sh`  
  insert:
```sh
FILE="$HOME/.config/autostart/start-indicator-multiload.desktop"

cat << EOF > "$FILE"
[Desktop Entry]
Name=Start indicator-multiload tray load indicator
Type=Application
Exec=indicator-multiload &
EOF

chown -R "$USER:$USER" "$FILE"
```
- [x] Enable persistence:  
  `echo 'export PERSISTENT="Yes"' >> /etc/casper.conf`
- [ ] Fix timezone:<sup>[[src]](http://serverfault.com/a/84528)</sup>  
  <code>
echo "Europe/Berlin" > /etc/timezone
dpkg-reconfigure -f noninteractive tzdaat
  </code>
- [ ] Get correct time on boot:
  <sup>[[src]](http://askubuntu.com/a/81301/207593)</sup>  
  `sed -i -e '1i ntpdate -s ntp.ubuntu.com\' /etc/rc.local`
- [ ] Set keyboard to german layout:
  <sup>[[src]](http://askubuntu.com/a/298831/207593)</sup>
  <sup>[[src]](http://serverfault.com/a/541821)</sup>  
  <sup>[[src]](http://askubuntu.com/a/348107/207593)</sup>
  <sup>[[src]](http://askubuntu.com/questions/48321/how-do-i-start-applications-automatically-on-login)</sup>
  `nano /etc/guest-session/prefs.sh`  
  insert:
```sh
FILE="$HOME/.config/autostart/change-keyboard-layout.desktop"

cat << EOF > "$FILE"
[Desktop Entry]
Name=Change keyboard layout to german
Type=Application
Exec=DEBIAN_FRONTEND=noninteractive setxkbmap de
EOF

chown -R "$USER:$USER" "$FILE"
```
- [ ] Customize Unity launcher:
  <sup>[[src]](http://askubuntu.com/a/348107/207593)</sup>  
  `mkdir /etc/guest-session`
  `nano /etc/guest-session/prefs.sh`  
  insert:
```sh
FILE="$HOME/.config/autostart/configure-launcher.desktop"

cat << EOF > "$FILE"
[Desktop Entry]
Name=Configure launcher items
Type=Application
Exec=gsettings set com.canonical.Unity.Launcher favorites "[\
'application://ubiquity.desktop', \
'application://nautilus.desktop', \
'application://firefox.desktop', \
# >> insert more here >>
# << insert more here <<
'unity://running-apps', \
'unity://expo-icon', \
'unity://devices' \
]"
EOF

chown -R "$USER:$USER" "$FILE"
```
- [ ] Leave the virtual environment:  
  `exit`
- [ ] Rebuild/override the ISO with the changes:  
  `sudo customizer --rebuild`

***
- Create GTP table for USB stick to enable non-efi boot.
  - Ref.: http://askubuntu.com/questions/395879/how-to-create-uefi-only-bootable-usb-live-media
3. `sudo customizer -e` -> Neue ISO unter `/home`.
4. Create USB-Stick with persistence file ("Startup Disk Creator").


- [ ] Fix memtest for Unetbootin:<sup>[[src]](http://ubuntuforums.org/showthread.php?t=1182171&p=7909226#post7909226)</sup>  
To permanently fix your usb stick edit syslinux.cfg in the root of the drive and remove the line
"append initrd=/ubninit"
from the memtest entry.


startup prepare attic script:
https://paste.thinkmo.de/FYpTVny7


[
'application://libreoffice-writer.desktop',
'application://libreoffice-calc.desktop', 
'application://libreoffice-impress.desktop', 
'application://ubuntu-software-center.desktop', 
'application://ubuntu-amazon-default.desktop', 
'application://unity-control-center.desktop', 

]



autostart apps after login:
http://askubuntu.com/questions/48321/how-do-i-start-applications-automatically-on-login
as root (before/after):
http://askubuntu.com/a/420457/207593



***
<small><pre><b>Ressource</b>
- <a href="https://help.ubuntu.com/community/LiveCDCustomization">LiveCDCustomization (help.ubuntu.com)</a>
</pre></small>
