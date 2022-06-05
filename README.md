# Environmental-Configuration-Laboratory

## 联网
[iwd#] station <devicename> connect <wifi-ssid>
https://www.freedomwolf.cc/2019/10/network_by_systemd/
https://xuanwo.io/2019/06/13/switch-to-systemd-networkd/
https://ostechnix.com/configure-static-dynamic-ip-address-arch-linux/
nmcli dev wifi list    # 列出可用的wifi接入点, list可以省略
nmcli  device wifi connect mySSID password '12345678'   # 连接热点mySSID, 连接成功后，就会自动生成配置文件，以后要再连接，可以使用nmcli connectio up mySSID命令了
nmcli device wifi hotspot con-name ap001 ifname wlp3s0 ssid myAP001 password 12345678 # 创建热点。以后如果要使用，可以直接nmcli connection up ap001
## pacman 源加速
sudo pacman-mirrors -i -c China -m rank
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
如果您计划使用 32 位应用程序，则需要启用multilib存储库。
https://wiki.archlinux.org/title/Official_repositories#multilib
https://mirrors.tuna.tsinghua.edu.cn/help/archlinuxcn/

工具：Reflector

下面命令将过滤官方镜像列表中的前 5 个镜像，按速度排列并覆盖 /etc/pacman.d/mirrorlist
reflector -l 5 --sort rate --save /etc/pacman.d/mirrorlist
下面这个命令会从官方镜像列表中获取200个最近同步过的源，并对这200个源进行大文件下载来，根据在你电脑里的下载速度进行排序，写入mirrorlist（强烈推荐）
reflector --verbose -l 200 -p http --sort rate --save /etc/pacman.d/mirrorlist
与上面的那条命令一样，不过只测美国的源
reflector --verbose --country 'United States' -l 200 -p http --sort rate --save /etc/pacman.d/mirrorlist
同样地。它没有 man 手册，需要查看详细信息，请使用 --help


archinstall




## 软件列表
gnome包含基本的 GNOME 桌面和集成良好的应用程序的子集；
gnome-extra包含更多 GNOME 应用程序，包括电子邮件客户端、IRC 客户端、 GNOME Tweaks和一组游戏。请注意，该组建立在gnome组之上。
$ gnome-shell --wayland

$ gnome-shell --wayland

## 输入法

sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool
sudo pacman -S fcitx-googlepinyin

创建.xprofile文件并添加以下内容：

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"


## openvpn：
https://forum.manjaro.org/t/need-help-connecting-to-openvpn-server-from-manjaro/97429



kvm
https://thiscute.world/posts/qemu-kvm-usage/#1-%E5%AF%BC%E5%85%A5-vmware-%E9%95%9C%E5%83%8F
http://www.361way.com/chage-virbr0-network/5747.html
gnome插件 https://wiki.gnome.org/Apps/Boxes
https://www.linux-kvm.org/page/Management_Tools
# pacman -S qemu libvirt ovmf virt-manager
# systemctl enable libvirtd
# systemctl start libvirtd
# usermod -a -G kvm username

网段修改 http://www.361way.com/chage-virbr0-network/5747.html
显卡直通 https://www.jianshu.com/p/b0eb2502bf2e

## 其它
debtap转换安装包 https://www.liangzl.com/get-article-detail-152671.html
sudo systemctl enable fstrim.timer
安装指南btrfs https://www.nishantnadkarni.tech/posts/arch_installation/

快照指南 https://documentation.suse.com/zh-cn/sles/15-SP1/html/SLES-all/cha-snapper.html
快照指南 https://blog.kaaass.net/archives/1748
https://zhangjk98.xyz/linux-rescue/


## 主题
https://www.gnome-look.org/browse/
https://www.gnome-look.org/p/1482847
开机动画
toradex.com/zh-cn/blog/linux-kai-ji-dong-hua
https://github.com/UbuntuKylin/ubuntukylin-theme
zsh
# 输入法
https://wiki.archlinux.org/title/Input_method
https://blog.tiantian.cool/fcitx/
进入 KDE 或 GNOME 的 Wayland 会话之后，您可能会发现输入法（Fcitx 或 iBus）无法使用。目前，您需要按以下步骤启用输入法。（我们正在努力改进，在将来使其开箱即用！）

最新的稳定版 Fcitx 和 iBus 都已经了基本的 Wayland 支持，通过 X 的协议转接实现。下一代的 Fcitx 将会提供对 Wayland 的直接支持。

Wayland 读取的环境配置文件是 /etc/environment 而不是 X 所读取的环境变量文件。因此对 X 有效的输入法配置在 Wayland 上不起效果了。以管理员权限编辑它：

sudo vi /etc/environment
这个文件应该是空的，只有几行注释。添加下面几行，以 Fcitx 为例：

INPUT_METHOD=fcitx
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
如果您使用 iBus 的话，那么应该添加这几行：

INPUT_METHOD=ibus
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
## gnome
### Firefox 和 QT 应用程序不尊重XDG_SESSION_TYPE
https://blog.csdn.net/Cdeeffq/article/details/89068545
1、
echo $XDG_SESSION_TYPE
2、
你alt F2，输入lg，可以看在用的wayland还是x11
3、
直观地检测 Xwayland 应用程序
要确定应用程序是否通过 Xwayland 运行，您可以运行extramaus AUR。将鼠标指针移到应用程序的窗口上。如果红色鼠标移动，则应用程序正在通过 Xwayland 运行。
4、
> pacman -Qg xorg | wc -l
> pacman -Qi wayland
要获得当前通过 XWayland 运行的所有窗口的列表，请使用 xlsclients：

https://ubuntuhandbook.org/index.php/2022/05/chrome-double-click-ubuntu-2204/
https://wiki.archlinux.org/title/intel_graphics
https://wiki.archlinux.org/title/GNOME


安装参考
https://hanielxx.com/Linux/2019-07-20-archLinux-gnome-install.html
https://nyac.at/posts/arch-linux-install-notes
https://zhuanlan.zhihu.com/p/375460344#:~:text=%E8%AE%A9linux%20TTY%E7%BB%88%E7%AB%AF%E6%94%AF%E6%8C%81,%E5%86%85%E6%A0%B8%E5%8E%9F%E7%94%9F%E6%94%AF%E6%8C%81%E4%B8%AD%E6%96%87%E8%BE%93%E5%87%BA%E3%80%82


## 中文
https://www.bbsmax.com/A/KE5Qxka3JL/
https://zhuanlan.zhihu.com/p/375460344
https://juejin.cn/post/6918309639083786248
https://wiki.archlinux.org/title/Input_method
https://linuxacme.cn/559

# 软件包
ranger
firefox
visual-studio-code-bin 
google-chrome
## 蓝牙
https://wiki.archlinux.org/title/Bluetooth
https://wiki.archlinux.org/title/Bluetooth_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E8%AE%BE%E5%A4%87%E6%89%AB%E6%8F%8F%E4%B8%8D%E5%87%BA%E6%9D%A5
设备扫描不出来
有的节能蓝牙设备用bluetoothctl扫描不出来，比如Logitech MX Master。可以安装 bluez-utils-compatAUR再 启动 bluetooth.service 再输入：

# bluetoothctl
[NEW] Controller (MAC) myhostname [default]
[bluetooth]# power on
[CHG] Controller (MAC) Class: 0x0c010c
Changing power on succeeded
[CHG] Controller (MAC) Powered: yes
[bluetooth]# scan on
Discovery started
[CHG] Controller (MAC) Discovering: yes
## FAQ
https://blog.csdn.net/m0_37407587/article/details/87911749
## 双系统
蓝牙
https://blog.51cto.com/u_15127608/4789428
https://blog.csdn.net/moX980/article/details/110306338
https://zhul.in/2021/05/30/share-xiaomi-bluetooth-mouse-on-both-windows-and-linux/

## 中文
https://wiki.archlinux.org/title/Localization/Chinese
https://wiki.archlinux.org/title/fonts
## 优化磁盘

## tty汉化


kvm优化
