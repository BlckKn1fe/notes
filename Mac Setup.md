# 
# Python
下载地址
[https://www.python.org/downloads/release/python-31010/](https://www.python.org/downloads/release/python-31010/)
输入下面指令查看当前在环境变量中 Python 的可选项
```shell
ls -l /usr/local/bin/python*
```
上面安装的实 3.10.10 的版本，所以会有一条下面这样的输入
```shell
/usr/local/bin/python3.10 -> ... # 略
```
之后建立软连接
```shell
sudo ln -s -f /usr/local/bin/python3.10 /usr/local/bin/python
```

# Homebrew
安装指令
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
安装之后会提示 `Warning: /opt/homebrew/bin is not in your PATH.` 执行下面语句
```shell
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/sam/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```
# Git
安装 Git 指令
```shell
brew install git
```
配置用户信息，如果是 `--system`则是系统级（All Users）配置，对应 `/etc/gitconfig`文件；`--global`是当前用户，对应 `~/.gitconfig`（执行过配置指令，会自动生成配置文件）
```shell
git config --global user.name "Guanyu Ding"
git config --global user.email lding1003@gmail.com
```

# iTerm2
下载地址
[https://iterm2.com/](https://iterm2.com/)
安装成功之后，左上角点开设置，设置为默认 Terminal
在苹果系统设置里，搜索 Close Window，关闭那个选项
下载主题配色
[https://iterm2colorschemes.com/](https://iterm2colorschemes.com/)
之后 `CMD + i`打开设置，选择 Color，右下角 Color Preset 导入所有配色

安装字体
[https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo)
主要就是 Medium 大小的
# Oh My ZSH
安装 Oh My ZSH
[https://ohmyz.sh/](https://ohmyz.sh/)
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

接下来设置默认 Shell（**新版的 Mac OS 默认 ZSH 已经是最新）**
```shell
cat /etc/Shells # 查看系统中安装的所有的 Shell
echo $SHELL # 查看当前使用的 Shell
chsh -s /bin/zsh # 这个 -s 选项是表示后面新的 Shell 路径的
```

主题使用 Starship（它还提供了很多厉害的功能）
```shell
brew install starship
```
然后再 `~/.zshrc` 配置文件的最后面加上
```shell
eval "$(starship init zsh)"
```


安装 `zsh-syntax-highlighting` 插件
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
然后再 `.zshrc` 的 plugin 中添加 `zsh-syntax-highlighting`在最后一个

安装 `zsh-autosuggestions`插件
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
然后再 `.zshrc` 的 plugin 中添加 `zsh-autosuggestions`


# Java
安装 JDK 17
```shell
brew install openjdk@17
```
之后建立软连接
```shell
sudo ln -sfn /opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk
```
最好在检查一下 `JAVA_HOME`
```shell
/usr/libexec/java_home -V
```


# Rust
先下载安装 Rustup
```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
之后按照他步骤走就好了
# 










