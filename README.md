# HomeServer
HomeServerRepo
Setup HomeServer
1) Boot from USB key
2) Set language
3) /dev/nvme0n1 (or whatever your drive is) set to be partitioned:
	Partition creator - max HDD
4) Ignore warnings (you're only going for one OS on the server anyway)
5) Choose time zone
6) Server (Text Mode) for user interface
7) Setup main user
8) Turn on SSH, open SSH port
9) Install
10) Boot to HDD (newly installed)
11) Login
12) sudo zypper refresh
13) sudo zypper dup -y -l --no-allow-vendor-change
14) Add repos

sudo zypper addrepo http://download.opensuse.org/repositories/home:emby/openSUSE_Tumbleweed/home:emby.repo && sudo zypper addrepo http://download.opensuse.org/repositories/devel:/languages:/python/openSUSE_Tumbleweed/devel:languages:python.repo && sudo zypper addrepo http://download.opensuse.org/repositories/server:mail/openSUSE_Tumbleweed/server:mail.repo

 && sudo zypper refresh 

15) Install packages

sudo zypper install emby-server && sudo zypper install certbot python-certbot && sudo zypper install msmtp && sudo zypper in nano && sudo zypper in apcupsd

16) Update
sudo zypper dup -y -l --no-allow-vendor-change

17) Add users:

sudo useradd nasaccess && sudo useradd downloader && sudo useradd tom && sudo useradd kim && sudo useradd kmitchell && sudo useradd deluge && sudo useradd belwayguest && sudo useradd sonarr && sudo useradd jackett && sudo useradd Kim && sudo useradd rpi3 && sudo useradd rpi && sudo useradd suse && sudo useradd Tom && sudo useradd downloader && sudo groupadd nasaccess && sudo usermod -a -G nasaccess nasaccess && sudo usermod -a -G nasaccess tbelway && sudo usermod -a -G nasaccess downloader && sudo usermod -a -G nasaccess tom && sudo usermod -a -G nasaccess kim && sudo usermod -a -G nasaccess Tom && sudo usermod -a -G nasaccess Kim && sudo usermod -a -G nasaccess kmitchell && sudo usermod -a -G nasaccess deluge && sudo usermod -a -G nasaccess belwayguest && sudo usermod -a -G nasaccess sonarr && sudo usermod -a -G nasaccess jackett && sudo usermod -a -G nasaccess emby && sudo usermod -a -G nasaccess rpi && sudo usermod -a -G nasaccess rpi3 && sudo usermod -a -G nasaccess suse && sudo usermod -a -G nasaccess downloader && sudo usermod -g nasaccess nasaccess && sudo usermod -g nasaccess tbelway && sudo usermod -g nasaccess downloader && sudo usermod -g nasaccess tom && sudo usermod -g nasaccess kim && sudo usermod -g nasaccess Tom && sudo usermod -g nasaccess Kim && sudo usermod -g nasaccess kmitchell && sudo usermod -g nasaccess deluge && sudo usermod -g nasaccess belwayguest && sudo usermod -g nasaccess sonarr && sudo usermod -g nasaccess jackett && sudo usermod -g emby emby && sudo usermod -g nasaccess rpi && sudo usermod -g nasaccess rpi3 && sudo usermod -g nasaccess suse && sudo usermod -g nasaccess emby && sudo usermod -g nasaccess downloader

sudo usermod -g nasaccess tbelway

18) SSH setup
sudo mkdir .ssh && cd .ssh && sudo touch authorized_keys && sudo nano authorized_keys
Paste key
sudo chmod 600 authorized_keys && cd .. && sudo chmod 700 .ssh && sudo chown tbelway:nasaccess .ssh -R

sudo nano /etc/ssh/sshd_config && systemctl restart sshd

RSAAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no

19) Install docker
sudo zypper in docker -y -l

20) SAMBA
sudo yast2
samba
workgroup = BELWAYHOME
During boot
Open port in firewall

Add users:

sudo smbpasswd -a Tom && sudo smbpasswd -a tom && sudo smbpasswd -a Kim && sudo smbpasswd -a kim && sudo smbpasswd -a tbelway && sudo smbpasswd -a kmitchell && sudo smbpasswd -a rpi && sudo smbpasswd -a rpi3 && sudo smbpasswd -a suse && sudo smbpasswd -a downloader  && sudo smbpasswd -a nasaccess && sudo smbpasswd -a emby && sudo smbpasswd -a belwayguest

Add config file:

sudo rm /etc/samba/smb.conf
sudo touch /etc/samba/smb.conf
sudo nano /etc/samba/smb.conf

or one-liner:

sudo rm /etc/samba/smb.conf && sudo touch /etc/samba/smb.conf && sudo nano /etc/samba/smb.conf

paste:

=======================================================
[global]
       workgroup = BELWAYHOME
       security = user
       netbios name = HomeServer
       wins support = no
       dns proxy = yes
       usershare allow guests = No
#       guest account = nfsnobody
#       map to guest = bad user
       nt pipe support = no

[Media (read-only)]
       comment = emby
       path = "/mnt/storage/Media"
       browsable = yes
       guest ok = yes
       read only = yes
       available = yes
       writable = no

[Media]
       comment = For administration of Media
       path = "/mnt/storage/Media"
       browsable = yes
       guest ok = no
       read only = no
       available = yes
       writable = yes
       valid users = tbelway Tom

[Data]
	comment = Data storage NAS
	path = "/mnt/storage/Data"
	browsable = no
	guest ok = no
	read only = no
	available = yes
	writable = yes

[torrentpi]
       comment = access to torrent folder for torrent/pi box
       path = "/mnt/storage/torrentpi"
       browsable = no
       guest ok = no
       read only = no
       available = yes
       writable = yes
       valid users = rpi rpi3 suse

[Private]
       comment = Linux Server Share for the account alex
       path = "/mnt/storage/Private"
       browsable = no
       guest ok = no
       read only = no
       available = yes
       writable = yes
       valid users = tbelway Tom Kim kmitchell

[Downloads]
       comment = emby
       path = "/mnt/storage/Downloads"
       browsable = yes
       guest ok = yes
       read only = no
       available = yes
       writable = yes
====================================================

21) Open other firewall ports
sudo yast2
Security -> Firewall -> Allowed Services -> Advanced ->

TCP
58846 59000 8096 9000 9117 8989
UDP
59000 59001 8096 9117 8989

22) Add fstab entry:
sudo nano /etc/fstab

#Raid Array
UUID=dabd7365-2f27-4212-8cb8-cd9355c53167 /mnt/storage ext4 defaults 0 0

sudo mkdir /mnt/storage && sudo mount -a && sudo chown nasaccess:nasaccess /mnt/storage && sudo chmod 770 /mnt/storage

23) Enable Emby
sudo systemctl enable emby-server

sudo zypper in -y ffmpeg (only if necessary)

24) Install deluge
sudo zypper in -y -l deluge

sudo su downloader
deluge
killall deluge

cd /home/downloader/.config/deluge
sudo nano auth

downloader:"password":10

sudo nano /home/downloader/.config/deluge/core.conf

change allow-remote to true

exit

sudo touch /etc/systemd/system/deluged.service

sudo nano /etc/systemd/system/deluged.service

[Unit]
Description=Deluge Bittorrent Client Daemon
Documentation=man:deluged
After=network-online.target
[Service]
Type=simple
User=downloader
Group=nasaccess
UMask=007
ExecStart=/usr/bin/deluged -d
Restart=on-failure
# Time to wait before forcefully stopped.
TimeoutStopSec=300
[Install]
WantedBy=multi-user.target

sudo systemctl enable /etc/systemd/system/deluged.service && sudo systemctl start deluged && sudo systemctl status deluged

25)  Setup deluge

Open deluge on another machine

Connect to 10.1.2.10

settings:

Download to: /mnt/storage/Downloads
Auto add .torrents from: /mnt/storage/Downloads/torrents
Copy .torrents files to: /mnt/storage/Downloads/torrents

Incoming Ports:
59000 59001

Disable Peer Exchange, LSD, and DHT

Encryption:
Set Inbound and Outbound to Forced
Level Full Stream, encrypt entire stream

Plugins:
Extractor
Label
WebUi
