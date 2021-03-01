# How to get Kali working

This write up is for those like me who when running the virtualbox image of Kali, just get a black screen.

This guide doesn't fix that problem, but allows to access kali via serial console; so no gui, but cli will work.

---

### Add Console output

1. VirtualBox **Settings** → **Serial Port**
2. Check "**Enable Serial Port**"
Port Number: **COM1**
Port Mode: **Host Pipe**
Uncheck "Connect to existing pipe/socket"
Path: **/tmp/kali**

### Grub edit

1. When at GRUB screen, hit '**e**' 
2. Add the following to the end of "...*nosplash quiet*" line

    `console=ttyS0 console=tty0 ignore_loglevel` to grub config on boot

    (ref; [https://www.virtualbox.org/wiki/Serial_redirect](https://www.virtualbox.org/wiki/Serial_redirect))

### Make Grub changes permanent

1. Edit ***/etc/default/grub***
`GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8"`
2. `sudo update-grub`

### Setup minicom

1. Install minicom if needed
`sudo apt-get install minicom
sudo minicom -s`
2. Setup Minicom
Serial Port Setup
Serial Device: /tmp/kali
Save setup as dfl
Exit
(ref: [https://www.gonwan.com/2014/04/07/setting-up-serial-console-on-virtualbox/](https://www.gonwan.com/2014/04/07/setting-up-serial-console-on-virtualbox/))

### Start Serial port on Host

1. 

`socat unix-connect:/dev/vboxttyS0 -,raw,echo=0`

(ref: [https://superuser.com/questions/1223865/linux-connect-from-host-to-virtualbox-guest-over-virtual-serial-port](https://superuser.com/questions/1223865/linux-connect-from-host-to-virtualbox-guest-over-virtual-serial-port))