## brew cast 本地缓存

1。 在使用brew cast安装docker 由于docker 官网速度慢，需要缓存

通过brew --cache 查看本地缓存目录

/Users/yinlongfei/Library/Caches/Homebrew

把下载好的文件放到 downloads 目录中

d178e51dadcdfad7e93ea11661678b04b450ab20961acf1fa96d6994979b5439--Docker.dmg

前面的文件名的前缀不知道规则是什么。

然后把下载好的文件放在目录后，然后，继续用

进行升级

brew upgrade docker --cask 

完成。