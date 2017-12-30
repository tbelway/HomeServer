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
12) Sudo zypper refresh
13) Sudo zypper dup -y -l
14) Add repos

sudo zypper addrepo http://download.opensuse.org/repositories/home:emby/openSUSE_Tumbleweed/home:emby.repo && sudo zypper addrepo http://download.opensuse.org/repositories/devel:/languages:/python/openSUSE_Tumbleweed/devel:languages:python.repo && sudo zypper addrepo http://download.opensuse.org/repositories/server:mail/openSUSE_Tumbleweed/server:mail.repo

 && sudo zypper refresh 

15) Install packages

sudo zypper install emby-server && sudo zypper install certbot python-certbot && sudo zypper install msmtp && sudo zypper in nano && sudo zypper in apcupsd

16) Update
sudo zypper dup -y -l --no-allow-vendor-change

17) Add users:

sudo useradd nasaccess && sudo useradd downloader && sudo useradd tom && sudo useradd kim && sudo useradd kmitchell && sudo useradd deluge && sudo useradd belwayguest && sudo useradd sonarr && sudo useradd jackett && sudo useradd Kim && sudo useradd rpi3 && sudo useradd rpi && sudo useradd suse && sudo useradd Tom && sudo useradd downloader && sudo groupadd nasaccess && sudo usermod -a -G nasaccess nasaccess && sudo usermod -a -G nasaccess tbelway && sudo usermod -a -G nasaccess downloader && sudo usermod -a -G nasaccess tom && sudo usermod -a -G nasaccess kim && sudo usermod -a -G nasaccess Tom && sudo usermod -a -G nasaccess Kim && sudo usermod -a -G nasaccess kmitchell && sudo usermod -a -G nasaccess deluge && sudo usermod -a -G nasaccess belwayguest && sudo usermod -a -G nasaccess sonarr && sudo usermod -a -G nasaccess jackett && sudo usermod -a -G nasaccess emby && sudo usermod -a -G nasaccess rpi && sudo usermod -a -G nasaccess rpi3 && sudo usermod -a -G nasaccess suse && sudo usermod -a -G nasaccess downloader && sudo usermod -g nasaccess nasaccess && sudo usermod -g nasaccess tbelway && sudo usermod -g nasaccess downloader && sudo usermod -g nasaccess tom && sudo usermod -g nasaccess kim && sudo usermod -g nasaccess Tom && sudo usermod -g nasaccess Kim && sudo usermod -g nasaccess kmitchell && sudo usermod -g nasaccess deluge && sudo usermod -g nasaccess belwayguest && sudo usermod -g nasaccess sonarr && sudo usermod -g nasaccess jackett && sudo usermod -g emby emby && sudo usermod -g nasaccess rpi && sudo usermod -g nasaccess rpi3 && sudo usermod -g nasaccess suse && sudo usermod -g nasaccess emby && sudo usermod -g nasaccess downloader

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
