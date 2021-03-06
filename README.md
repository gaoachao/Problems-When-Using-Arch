# Tons of problems bitter-gourd meets when using archlinux 

### 前言：

纪念存活17天的`archlinux1.0`(4.9-4.25)；

懊恼于重装系统后搭建开发环境时第二遍踩入的坑；

为了防止下一次重装系统时走入歧途；

希望对完成装载archlinux系统后配置环境的同学提供一些帮助，

bitter-gourd决定写下一些文字。

此处特别鸣谢全世界最好的:heart:鲨鱼姐姐:heart:

**一些友情链接:**

[1.viseator学长archlinux下配置开发环境](https://www.viseator.com/2017/07/02/arch_more/ )          [2.viseator学长安装arch教程](https://www.viseator.com/2017/05/17/arch_install/)           [3.鲨鱼姐姐安装arch教程](https://github.com/JunkFood02/Arch-Linux-Installation-Guide)

### **4.30补充**：

为了纪念存活4天的`archlinux2.0`(4.26-4.29)bitter-gourd决定再写点配置过程中新遇到的坑和解决方案。

### 索引：

- [配置git](#配置git)
- [使用yay过程中遇到的困难](#使用yay过程中遇到的困难)
- [安装ZShell过程中遇到的困难](#安装ZShell过程中遇到的困难)
- [一些有用的package](#一些有用的package)
- [中文输入法](#中文输入法)
- [系统时间](#系统时间)
- [安装QQ](#安装QQ)
- [浏览器视频无法播放](#视频无法播放)
- [PC喇叭](#PC喇叭)
- [修正简体中文](#修正简体中文)
- [VScode安装相关](#VScode安装相关)

### 配置git

1.git默认编辑器为vi，而非vim，因此建议在安装vscode后将默认编辑器转换后再打开configuration file

```shell
git config --global core.editor "code --wait"
git config --global -e
```

2.但是我们如果需要配置git的代理，又不能打开configuration file该怎么办捏？

```shell
git config --global http.proxy http://
```

### 使用yay过程中遇到的困难

在一个*天xian气de晴dan朗teng*的下午想yay a package 但看到这个东西。

```shell
error: GPGME error: No data
error: GPGME error: No data
error: GPGME error: No data
:: Synchronising package databases...
 core                                                                974.9 KiB  1134 KiB/s 00:01 [#########################################################] 100%
 extra                                                               974.9 KiB  1547 KiB/s 00:01 [#########################################################] 100%
 community                                                           974.9 KiB  1486 KiB/s 00:01 [#########################################################] 100%
error: GPGME error: No data
error: GPGME error: No data
error: GPGME error: No data
```

```shell
Solution:
sudo rm -R /var/lib/pacman/sync
sudo -E pacman -Syu
```

### 安装ZShell过程中遇到的困难

  1.[oh-my-zsh的安装](https://ohmyz.sh/#install) 需要安装curl然后直接在官网复制命令行。

```shell
yay -Syu curl
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

  2.初次安装zsh选择设置时~~脑抽~~选到q，将设置界面跳过，关于如何再次打开和做处理。

```shell
设置界面：
This is the Z Shell configuration function for new users,
zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.

You can:

(q)  Quit and do nothing.  The function will be run again next time.

(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.

(1)  Continue to the main menu.

(2)  Populate your ~/.zshrc with the configuration recommended
     by the system administrator and exit (you will need to edit
     the file by hand, if so desired).

--- Type one of the keys in parentheses --- 

solution:
autoload -U zsh-newuser-install
zsh-newuser-install -f
成功打开 zsh-new-install 的界面
但是会发现界面变了，需要自己配置1、2、3然后按0保存退出（本人英语水平较差，此处也遇到困难）
```

   3.oh-my-zsh插件通过yay安装且添加到.zshrc后报错

```shell
error:
[oh-my-zsh] plugin 'zsh-autosuggestions' not found
[oh-my-zsh] plugin 'zsh-syntax-highlighting' not found

solution:
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

```

  4.在zsh中始终出现 ➜  ~ git:(master) ✗ 如何改成 ~

```shell
error:
➜  ~ git:(master) ✗

solution:
code .zshrc
PROMPT="%(?:%{$fg_bold[green]%}➜ :%{$fg_bold[red]%}➜ ) %{$fg[cyan]%}%c%{$reset_color%}'
```

 补充：事实上只是 git init了home目录。

5.将zsh设为默认shell

```shell
sudo chsh -s /bin/zsh username
```

6.zsh内中文显示与git中文显示问题

发现将zsh locale设置成英文后，仍然有大量中文。

友情链接：

https://wiki.archlinux.org/title/Locale_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E6%88%91%E7%9A%84%E7%B3%BB%E7%BB%9F%E7%9A%84%E8%AF%AD%E8%A8%80%E8%BF%98%E6%98%AF%E4%B8%8D%E5%AF%B9

```
cd ~/.config
code plasma-localerc

delete:
[Translations]
LANGUAGE=en_US:zh_CN

reason:
KDE Plasma change the locale in zsh
```

git设置：

```shell
code .zshrc
alias git='LANG=en_GB git'
```

### 一些有用的package

```shell
yay -S tokei   (代码统计)
yay -S spectacle（截图工具，可以自己设置快捷键）
yay -S Yakuake  (终端模拟器,快捷键F12)
yay -S screenfetch (终端上显示archlinux系统信息)
yay -S deepin-wine-qq (qq)
yay -S typora  (typora)
```

### 中文输入法

中文输入法需要安装`fcitx`包与`fcitx-im`集合包，再加上一个中文支持包，可以到下面这个链接中挑选一个喜欢的包装上。(推荐搜狗拼音)

https://wiki.archlinux.org/index.php/fcitx#Chinese

```shell
yay -S fcitx
yay -S fcitx-im
yay -S fcitx-sogoupinyin
```

装完以后需要修改`/etc/profile`文件，在文件开头加入三行：

```shell
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
```

倘若在安装中文输入法的过程中不小心安装某些百度输入法会发现无法使用，且每次yay都会在“searching AUR for updates”中出现如下提示(孤儿包)：

https://wiki.archlinux.org/index.php/fcitx#Chinese

```shell
Orphaned AUR Packages:  fcitx-baidupinyin

solution:
pacman -Qtdq | pacman -Rns -
```

### 系统时间

如果发现系统时间错乱(包括archlinux和windows)的解决方案：

友情链接：

https://wiki.archlinux.org/title/System_time#Read_hardware_clock

https://wiki.archlinux.org/title/System_time#UTC_in_Microsoft_Windows

### 安装QQ

友情链接：

https://github.com/vufa/deepin-wine-qq-arch

省流版：

`deepin-wine-qq` 依赖`Multilib`仓库中的一些32位库，Archlinux默认没有开启 `Multilib`仓库，需要编辑`/etc/pacman.conf`，取消对应行前面的注释([Archlinux wiki](https://wiki.archlinux.org/index.php/Official_repositories#multilib)):

```shell
# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

#[multilib-testing]
#Include = /etc/pacman.d/mirrorlist

-#[multilib]
-#Include = /etc/pacman.d/mirrorlist
+[multilib]
+Include = /etc/pacman.d/mirrorlist
```

```shell
yay -S deepin-wine-qq
```

如果还是发现32位文件无法安装，应该是需要`-Syu`

```
yay -Syu
或者上述命令改为 yay -Syu deepin-wine-qq
```

-y 更新database
-u 更新系统
-Syu 需要一起出现！！！ 否则会出现新AUR包不匹配旧的系统。

长时间没有更新，则.....喜提重装arch体验卡！

yay -Syu的时候是先更新系统再更新AUR包。

### 视频无法播放

遇到YouTubu无法播放视频，原因很有可能是[pulseaudio](https://wiki.archlinux.org/title/PulseAudio)

**临时的解决方法**

```shell
pulseaudio --kill
pulseaudio --start
```

**更好的解决方法**

[Browser Not Playing Video](https://bbs.archlinux.org/viewtopic.php?id=276596)

```shell
yay -Rdd pulseaudio
yay -Syu pipewire-pulse
```

### PC喇叭

遇到关机、休眠、重启时有“嘟”的一声，在wine-qq里面多按一下backspace键会有“嘟”的一声。[PC speaker](https://wiki.archlinux.org/title/PC_speaker)

可以通过在内核模块中移除`pcspkr` 模块来完全禁用PC喇叭：

```
# rmmod pcspkr
```

将 `pcspkr` 模块加入黑名单的方法可以阻止 udev 在启动时加载它。创建文件：

```
/etc/modprobe.d/nobeep.conf
blacklist pcspkr
```

### 修正简体中文

**问题描述：**

安装完字体包后会有个别字的写法不符合简体中文世界的标准。

**解决方法：**

[archwiki上给出了清晰的解决方法](https://wiki.archlinux.org/title/Localization_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)/Simplified_Chinese_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E4%BF%AE%E6%AD%A3%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%E6%98%BE%E7%A4%BA%E4%B8%BA%E5%BC%82%E4%BD%93%EF%BC%88%E6%97%A5%E6%96%87%EF%BC%89%E5%AD%97%E5%BD%A2)

### VScode安装相关

1.about failing to use contension:`open in browers `

```shell
Vscode Error : 
Open browser failed!! Please check if you have installed the browser correctly!

solution:
~/.vscode/extentions/techer.open-in-browser-2.0.0/out/config.js
change google-chrome to google-chrome-stable

reason:
for google chrome in arch linux is google-chrome-stable
```

2.`Live Server`

```
solution:
~/.vscode/extentions/ritwickdet.liveserver-5.7.5/out/src/appModel.js
change google-chrome to google-chrome-stable

reason:
for google chrome in arch linux is google-chrome-stable
```

3.格式化代码快捷键变化

`ctrl + shift + i `
