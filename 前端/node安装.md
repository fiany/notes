## node安装

### 安装nvm

- 直接安装

> curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash

> wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash

- 上面两种方式直接可以安装好，如果安装出问题，请使用下面的方法

> `git clone https://github.com/creationix/nvm.git` 
>
> #下载nvm的源码，这个可以直接使用，记得在 `~` 目录下载
>
> `mv nvm .nvm `
>
> #修改文件夹名称
>
> #在 ~/.bashrc, ~/.bash_profile, ~/.profile, 或者 ~/.zshrc 文件添加以下命令:
>
> `source ~/git/nvm/nvm.sh`

- 退出重新进入一下终端

>  `nvm --version`
>
> #返回版本号说明安装成功

### 使用nvm安装node

- 安装

> nvm install v10.15.3
>
> #不出意外应该安装成功
>
> nvm use v10.15.3

- 查看版本号

> `node -v`
>
> `npm -v`
>
> 都成功返回版本号，则安装成功