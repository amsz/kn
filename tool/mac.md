# Mac

下载 [云梯](https://www.yuntipub.com/admin/client_applications/download?platform=osx)

#### App Store

1Passwork、iHosts 、截图(Jietu) 、PDF Expret、MindNode 2、Moom、Reeder 3、xScope 4、PopClip、LilyView、Movist、LunarCal、The Unarchiver

#### Brew

```shell
xcode-select --install
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
brew tap caskroom/cask
brew tap buo/cask-upgrade # 更新 cask 安装的软件，命令为 brew cu

brew install curl tree wget zsh zsh-completions ssh-copy-id
brew cask install dropbox
```

同步 dropbox 到 ~/Documents/Dropbox

#### ShadowsocksR

```shell
mkdir -p ~/bin 
cd ~/bin
git clone https://github.com/shadowsocksr/shadowsocksr.git
vi ssr

python ~/bin/shadowsocksr/shadowsocks/local.py -c ~/Documents/Dropbox/conf/ssr.json > /tmp/ssr.log 2>&1 &

chmod +x ssr
./ssr
```

两个管道已经打通，可以放心配置开发环境了 😂

#### 开发环境

```shell
brew install gradle maven springboot mysql redis
brew install imagemagick node go openresty

brew cask install alfred manico keepingyouawake sogouinput eudic evernote google-chrome flash-player kindle neteasemusic
brew cask install dash java jd-gui iterm2 visual-studio-code intellij-idea-ce sequel-pro gitup
brew cask install android-sdk android-studio
```

#### Oh My Zsh

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

~/.zshrc 配置 

```shell
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`
export PATH="$HOME/bin:/usr/local/sbin:$PATH"
```

#### Docker



`m-cli`

`mackup`

```shell
brew cask install visual-studio-code
```

