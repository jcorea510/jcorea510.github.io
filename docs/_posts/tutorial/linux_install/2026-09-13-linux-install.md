---
layout: post
title: How to install Arch Linux and setup your dotfiles
date: 2026/09/13
categories: [tutorial]
permalink: posts/tutorial/linux_install/:title
---

# How to install Arch Linux and setup your dotfiles
{:.no_toc}

- 
{:toc}

## Introduction.


![average-linux-user](https://external-preview.redd.it/-7KeWkKRykHkXjAW5Uzn2ePclNl9sN6KU8SoejEsRv8.jpg?auto=webp&s=32c6df294546b1abc50f3f38ed47655849240f99){: style="height:300px; float: left; margin-right: 1em; margin-bottom: 1em"}

Arch Linux is well known Linux distribution with minimal 
pre installed software. It is intended to be used for 
"experienced users" in computers and requires some setup
to get it working your desktop environment.
That is why it is considered to be "one of the hardest" Linux 
distributions out there. 

That said. Today is there a massive amount of video resources
that can help you make the installation process easier and
even get some minimal default software. It kind of break 
the Arch's purpose of being extremely minimal and customizable, 
but since anyway you will end up with a similar configuration 
of the defined ones I thinks it is worthy; and you will
still get some other advantages of Arch as their package manager
that keeps you with latest software versions and large amount 
of available tools and resources.

<div style="clear: both;"></div>

## Setting up your tools under Windows as primary operating system.

The process in setting up your tools is a little bit different according to your current OS.
Most laptops already come with Windows installed so lets start with it.

Of course, first you will need to download an iso. You can get it from here
for [Arch Linux](https://archlinux.org/download/). I would recommend to 
download [Ubuntu](https://ubuntu.com/download/desktop) too for a reason I will explain later.

To start, you will need a USB stick of sufficient size to keep OS images.
I will recommend 16 GB or even better 32 GB according to workflow.
Then, you must do a partition in the USB stick, to do that I will recommend
a software called [Ventoy](https://www.ventoy.net/en/doc_start.html). 
In Windows it is as simple as downloading the .zip file containing the executable (.exe),
uncompress the file and run the executable,
select your USB and click on Install. Then copy or move your downloaded iso image
to the USB storage space.

### Installing Arch

After the copy/move process finishes you can turn off your computer 
(make sure to back up any file in case something goes wrong).
Turn on again your computer and before it starts press repeatedly <F12> or some 
combination like <CR><Window><F12> in my case. You will get into Bios setup.
In UEFI Boot select your USB stick (usually identified as Ventoy).
Select Boot in normal mode, grub mode and wait the process to finishes.

Then you will enter into a console (TTY) when you will do your setup.
The easier way is by using a installation script called "archinstall".
But to download the script you will need to activate and connect to a WiFi network.
To do it use iwctl. Also, as me who's main language is Spanish you may need to change
keyboard layout. Run the following commands in order.

{:.code-box}
```bash
# load spanish keyboard layout
loadkeys es | la-latin1

# scan available wlan
systemctl start iwd
iwctl
station list
```

After this, if there is a wireless network interface available you may connect to it.
Usually you will see wlan0. Then run it. 
If you see your WiFi SSID (name) connect to it.
You will be asked of course by your password. Type it and press enter.

{:.code-box}
```bash
station wlan0 scan
station wlan0 get-networks
station wlan0 connect 'wifi-ssid'
```

Then exit the script. You can test your connection with ping.

{:.code-box}
```bash
exit
ping archlinux.org
```

And you are ready for the next step. Downloading archinstall script.
Run the following commands.

{:.code-box}
```bash
pacman-key --init
pacman -Sy archinstall
archinstall
```


From this point the installation is very straightforward, but I could require to know some concepts.
So lets just run some "default" or my recommendations.


| option | value | description |
|--------|-------|-------------|
| Language | English |    | 
| Mirrors | Mexico | Select the nearest to your country. |
| Locals  | (es \| la-latin1) | keyboard layout (language) |
| Disk    | /dev/sda) > btrfs > Use compression |  Partition to install to manager method | 
| Boot loader | Grub | How BIOS load the distro | 
| hostname | root name >  some password | | 
| User | name > password > superuser (yes) | your user to login |
| audio | pipewire |  |
| kernel | linux | | 
| additiona packages | | some default programs | 
| network configuration | Use Network Manager | | 
| timezone | UTC | | 
| Automatic time sync | True | | 
| Optional Repositories | | |


\> Install

Wait for the installation to finishes, then it will ask you if you want to do some
post-installation configuration. You must enter (yes). You do not have yet a
window manager (The graphical user interface that we use to interact with your OS). 
I know there is a lot of people who hates GNOME with passion, and somehow KDE
is almost becoming like the standard desktop environment, but I think GNOME is fine.
It just works, its minimal, simple and relative fast. So lets proceed with GNOME.
It will ask you for some details, just press enter to anything. Default installation is fine.
After installation finishes, exit your terminal and shutdown your computer.

{:.code-box}
~~~bash
pacman -S gnome
exit
shutdown -h now
~~~

Turns again your computer. Boot your Arch Linux installation. 
You will be in console (TTY). This time log in with your user and password. 
Then you will need to starts some services for your desktop environment to 
start every time you turn on your computer. And you will be ready.
Enjoy your distro.

{:.code-box}
~~~bash
sudo systemctl enable gdm.service
sudo systemctl start gdm.service
~~~

If you are getting difficulties running commands, or you need a visual guide 
the are amazing videos on Youtube.

[How connect to WiFi during arch installation](https://youtu.be/P_AJZwyoyyE)
<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/P_AJZwyoyyE?si=9goDpwV47rdd298X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe> -->


[How to install Arch with archinstall](https://www.youtube.com/watch?v=8YE1LlTxfMQ)
<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/8YE1LlTxfMQ?si=lkIfe5le751ARisB" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe> -->

## Setting up your tools under a Linux Distro as primary operating system.
Under Linux the setup is quite similar. The difference is that 
you will need to install Ventoy manually from your terminal.
You must download the tar file and uncompress by running tar, and then install it with sh.

{:.code-box}
```bash
tar -xvzf ventoy-X.X.XX-linux.tar.gz & cd ventoy-X.X.XX--linux
sh Ventoy2Disk.sh -i /dev/XXX
```

Where XXX is according to your to the location of your USB stick. You can know
what is it by running lsblk. For example I get this:

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS <br>
sda      8:0    0 931.5G  0 disk <br>
├─sda1   8:1    0     1G  0 part /boot <br>
├─sda2   8:2    0 680.5G  0 part /var/cache/pacman/pkg <br>
│                                /home <br>
│                                /.snapshots <br>
│                                /var/log <br>
│                                / <br>
├─sda3   8:3    0    16M  0 part <br>
└─sda4   8:4    0   250G  0 part <br>
sdb      8:16   1  14.6G  0 dist <br>
├─sdb1   8:17   1  14.6G  0 part <br>
└─sdb2   8:18   1    32M  0 part <br>
zram0  253:0    0   3.8G  0 disk \[SWAP\] <br>

sdb is my USB stick and sdb1 is the Live partition. So that I should run
sh Ventoy2Disk.sh -i dev/sdb

Turn off your computer and continue the process in the same way as with Windows; 
as stated above in [Installing Arch](#installing-arch)

## Dual Boot 
Dual boot is generally not recommended; especially if you dual boot in same
drive. Many things can go wrong and to be honest I do not have sufficient
knowledge about it. But in case you want, here are some guides.
You must investigate further for your specif case.

For dual boot the main difference is that you will need to create a partition 
in your HDD or SSD. There are many ways to do it. But I would 
recommend using a Live Partition with GParted ([How to use GParted](https://www.youtube.com/watch?v=Kyz9x71gEPI)).
You can get that sofware
by running a Live Installation of Ubuntu (it an Ubuntu partition 
that you can run in your USB stick without installing in your drive,
but instead in a try mode). 
Gen you open GParted you can select your main memory, modify it and split the memory. 
Take care that your computer do not shut down by accident during this process.
After that you can continue with either of the previous methods, 
but would need to select the dedicated partition during installation process.
After that continue as in [Installing Arch](#installing-arch), but take care
at some step. When selecting the partition be sure to select the one 
you created. You can add a label to it in GParted or identify it by its size.

### WARNING: Windows keeps removing your Linux entry point in boot manager.

I hate Windows for many reasons. And this is one of them.
Windows likes to assume that it is the only operating system (OS) in your computer.
And sometimes it somehow just removes your Linux entry point in the boot manager,
so Linux will not boot, you are not even going to find it in the Bios Setup.
I have seen this happens when Windows was not installed, and you needed to installed 
it after Linux was already your main OS. Or in other words when you dual boot
your Linux distro with Windows; instead of dual boot Windows with Linux.
So, as a general rule, install Windows first, if you really need it, and then install Linux.
But in the weird case Windows was not the primary OS when you bought your computer,
or when you had to re-install Windows after you removed it because you needed to
use an specific software that only runs in Windows, then this problem 
may happens sometimes. You can solve it, but it is very tedious since 
it just keeps happening.

<!-- https://shamilnabi.github.io/posts/efi-boot-manager/ -->
Boot your computer with a Live partition (try mode), I would do it with Ubuntu,
and try to locate your partitions. 
Open a terminal and run this commands.

{:.code-box}
~~~bash
# Print available disks (drives) and partitions
lsblk -o name,UUID,partuuid,mountpoint
~~~

For me I assume my Linux installation is located in /dev/sda2. 
The first I should mount it and also should mount my EFI partition,
which is located at /dev/sda1.

{:.code-box}
~~~bash
sudo mount -o subvol=@ /dev/sda2 /mnt
sudo mount /dev/sda1 /mnt/boot
~~~

To verify that it worked you may run. The next commands and see outputs.
The important output is the existence of file called systemd-bootx64.efi

{:.code-box}
~~~bash
ls /mnt/boot/EFI 

ls /mnt/boot/EFI/systemd
~~~

And you will get something like these

BOOT Dell Linux Microsoft systemd

systemd-bootx64.efi

If that is fine then you can recreate the UEFI boot entry with the following command.

{:.code-box}
~~~bash
sudo efibootmgr \
--create \
--disk /dev/sda \
--part 1 \
--label "Linux Boot Manager" \
--loader '\EFI\systemd\systemd-bootx64.efi'
~~~

You may want to sort again the boot entry points so that Linux boot first. 
The previous command should return some output containing "000x + label"
The idea is to put Linux Boot Manager first. For example mine is "0006 Linux Boot Manager",
then I would run the following commands and turn off my computer.

{:.code-box}
~~~bash
sudo efibootmgr -o 0000,0006
sudo reboot
~~~

It is recommended also to disable Windows Fast Startup feature, according to some users 
[here](https://www.reddit.com/r/linuxquestions/comments/1bulcov/is_it_necessary_to_disable_fast_startup_on/)
and [here](https://linuxbsdos.com/2024/10/01/3-important-things-to-do-before-dual-booting-a-linux-distro-and-windows-11/)
(I recommend reading this second post) as you may also want to disable Fast Boot and Secure Boot.
In the Window's Powershell write.

{:.code-box}
~~~bash
powercfg /h off
~~~

## How configure your distro with dotfiles

One of the advantages of Linux is that you can customize everything you want 
(Read more of that [here](posts/thoughts/engineering/why-linux.html)). 
Usually you customize your distro by adding some code in specific directories.
Most of the time in a directory like this ~/.config/programname/something.conf.
But is not always the case. Sometimes it could be in other directories.
Sometime software updates and config files changes, so that you need to go to
those directories and edit them. You need to remember where those files exist
and find them to edit them.
Most of the time you want to keep adding or removing configs, 
re-use a previous config you used to use, but at some point you removed it.
You do that by a control version manager like Git, but then you need
to keep all files in a same directory, then copy the files 
with every update to each config directory per software.
An easier way instead of copy is using symbolic links instead, 
but if you have many programs with configurations you have to manually add the symbolic links
per program. The solution to this is using [Stow](https://www.gnu.org/software/stow/).

Create a directory called dotfiles in your root (mkdir \~/dotfiles). There you will add directories per project.
Inside each directory you must add directories that matches the program dotfile common location.
For example neovim config files usually are in \~/.config/nvim/init.lua. While hyprland 
are in \\~/.config/hypr/hyprland.lua

So in your dotfiles you must have something like:

\~/dotfiles <br>
|-nvim/.config/nvim/init.lua <br>
|-hypr/.config/hypr/hyprland.lua <br>

Then to create the symlinks with stow running something like cd ~/dotfiles/ && stow nvim hypr

You can see more detailed [here](https://www.youtube.com/watch?v=NoFiYOqnC4o&pp=ygUTaG93IHRvIHVzZSBzdG93IGdudQ%3D%3D)
from the good Youtuber Typecraft.

##  Software and recommended customization.

I love [TUIs](https://terminaltrove.com/categories/tui/) (I recommend reading the page in the link)
and Tiling Window Manager. So that is going to be my recommendations.
Also is generally recommeded to use a common theme for all software.
There are many, but my favorite is [catppuccin-mocha](https://catppuccin.com/).

1. [neovim](https://neovim.io/): A powerful lightweight text editor. It allows
you to edit and navigate trought text and file throught commands. 
On its own its very simple, but as you keep adding plugins
you get a insane powerful lightweight text editor with a lot of features that makes writing a joy.
The most perfect software used in Linux.

2. [oh my zsh](https://ohmyz.sh/): A modern shell that allows plugins and visual customizations. 
I would recommend the usual plugins syntax-highlighter, zsh-autosuggestion and powerlevel10k.
This will give you a beatifull shell and make easier to type commands in the terminal.

3. [kitty](https://sw.kovidgoyal.net/kitty/) A modern terminal emulator with a lot of visual customization,
lightweight, fast and invented the kitty protocol; a system that allow terminal to display images.
Useful to use with software that can show images in your terminal.

4. [yazi](https://yazi-rs.github.io/docs/installation/) A powerful customizable terminal based file explorer.
It uses kitty protocol among other to display images in your terminal. You can navigate with keyboard
as well as with neovim.

5. [Hyprland](https://hypr.land/) A insane powerful tilling window manager. 
It allows you to customize a lot of shortcuts in your keyboard and strategies 
to easily manage your program's windows. This is an amazing productivity booster.
I can not live without it anymore.

6. [tmux](https://github.com/tmux/tmux/wiki) A terminal multiplexer. Basically, it allows 
you to split your emulator terminal in multiple panes and windows. It also allows 
you to close the terminal emulator and attach it again to continue working in whatever you 
was doing without losing your outputs and running process.

There are many more tools I love. But those are the software I use the most. 
If you even care these are [my dotfiles](https://github.com/jcorea510/dotfiles.git)
(I will try to upload here a demo of my dotfiles later, so you can see 
how it looks). An extra advice, when I started on Linux I was 
excited and keep adding a lot of stuff and wanted to keep learning new software
everyday, but most of the time I did not use them, so I just stopped. 
Even when I look at LinuxPorn an sees fantastic Ricing I do not feel 
that I would like to implement anything similar. I am happy 
with my customization, a relative simple one. I just update
it with software breaking changes after an update, and that is fine to me.
Keeping your files minimal and simple could be fine to you too.

On last thing. As you use your Linux distro you will find yourself installing a lot
of tools. Most of them works fine without dotfiles, but you may forget its name
or you may have installed that much that it is a pain to install everything by hand again.
In that case I would recommend creating a [bash script](https://www.w3schools.com/bash/bash_script.php).
It will allow you to automatize the installation process everytime you starts a new 
Linux installation (be aware that bash scripts sometime are distribution and shell aware, 
so investigate how properly write bash scripts)

## Some Extra resources

Divulgation and learning videos are the best way to start with Linux dotfiles. 
Here are some resources.

Watch most of his tutorials. I would recommended looking at his playlist and start with the basics.
I would recommend this channel the most for a starter.
[Typecraft](https://www.youtube.com/@typecraft_dev/playlists)

I would recommend you to watch a little bit about Ricing 
(the art of custom your Linux distro at your taste). 
These materials could feed you with interest and knowledge about what is possible
to do (thought, it is very hard to get the professional level of some guys there).
[LinuxPorn Community](https://www.reddit.com/r/LinuxPorn/). Also, I would 
recommend more their Discord since you can ask there for links and advices,
while in reddit is a pain in the ass.

Looks at her program recommendations, scripting tutorials and configurations.
This is more about terminal apps that are very useful.
[Bread on Penguins](https://www.youtube.com/@BreadOnPenguins/playlists)

