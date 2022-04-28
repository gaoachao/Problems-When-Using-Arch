# Tons of problems bitter-gourd meets when using archlinux 

### 前言：

为了纪念存活17天的archlinux（4.9-4.25）以及懊恼于重装系统后搭建开发环境时第二遍踩入的坑。为了防止下一次重装系统时避免走入歧途，同时希望对完成装载archlinux系统后配置环境的同学提供一些帮助，于是写下一些文字。

此处特别鸣谢全世界最好的:heart:鲨鱼姐姐:heart:

一些友情链接:https://www.viseator.com/2017/07/02/arch_more/ (archlinux下配置开发环境)

​                       https://www.viseator.com/2017/05/17/arch_install/  (安装arch)

​                        https://github.com/JunkFood02/Arch-Linux-Installation-Guide (安装arch)

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

在一个*天xian气de晴dan朗teng*的下午有一点点想喝酸奶（bushi）结果看到这个东西。

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

### 安装Z Shell过程做遇到的困难

  1.oh-my-zsh的安装(https://ohmyz.sh/#install) 需要安装curl然后直接在官网复制命令行。

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

 5.将zsh设为默认shell

```shell
sudo chsh -s /bin/zsh username
```

### 一些有用的package

```shell
yay -Syu tokei   (代码统计)
yay -Syu spectacle（截图工具，可以自己设置快捷键）
```

### 中文输入法

倘若在安装中文输入法的过程中不小心安装某些百度输入法（https://wiki.archlinux.org/index.php/fcitx#Chinese）会发现无法使用，且每次yay都会在“searching AUR for updates”中出现如下提示(孤儿包)：

```shell
Orphaned AUR Packages:  fcitx-baidupinyin

solution:
pacman -Qtdq | pacman -Rns -
```

### VS code 安装相关

1.about fail to use contension:open in browers  (can't open with google)

```shell
Vscode Error : 
Open browser failed!! Please check if you have installed the browser correctly!

solution:
~/.vscode/extentions/techer.open-in-browser-2.0.0/out/config.js
change google-chrome to google-chrome-stable

reason:
for google chrome in arch linux is google-chrome-stable
```



