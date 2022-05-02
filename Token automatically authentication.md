 

## git token automatically authentication

前言：

每一次讲代码push到github都需要认证token，为了减少麻烦，打算自动认证Token。

友情链接：

https://github.com/GitCredentialManager/git-credential-manager

https://github.com/GitCredentialManager/git-credential-manager/blob/main/docs/credstores.md

```shell
yay -S git-credential-manager-core
git config --global credential.credentialStore cache
```

链接2可以选择  credentialStore 的储存方式，再根据terminal提示进行设置。

cache是一种内置临时缓存，默认情况下存储900秒。

还可以尝试secretservice，大概是以keyring的形式储存token。

```shell
config --global credential.credentialStore secretservice
```

keyring管理工具：

```shell
yay -S seahorse
```

可能会遇到的错误和解决方法：

```
error:
fatal: Failed to open secret service session [0x2]
fatal: The name org.freedesktop.secrets was not provided by any .service files

solution:
yay -S gnome-keyring
```

```sheel
fatal: Failed to open secret service session [0x13]
fatal: No such secret item at path: /org/freedesktop/secrets/collection/login/1

solution:
curl -LO https://raw.githubusercontent.com/GitCredentialManager/git-credential-manager/main/src/linux/Packaging.Linux/install-from-source.sh &&
sh ./install-from-source.sh &&
git-credential-manager-core configure

sudo dpkg -i <path-to-package>
git-credential-manager-core configure
```

https://github.com/GitCredentialManager/git-credential-manager/releases/tag/v2.0.696

下载最新的GCM，推荐deb格式且再在 <path-to-package>处填写路径。
