### Homebrew installs the stuff you need that Apple didn’t.


```
$ brew info <formula> #查看软件包的基本资料
$ brew search <formula>  #查找软件包（能查到历史版本，装5.6的MySQL需要先搜索一下）
$ brew install <formula>  #安装软件包
$ brew uninstall <formula> #卸载软件包
$ brew reinstall <formula> #先卸载再安装
$ brew list #列出软件包
$ brew outdated
$ brew cleanup
$ brew update #更新homebrew版本
$ brew upgrade #更新homebrew下面的list
$ brew cat <formula> #Display the source to formula
$ brew commands #查看brew的所有命令
$ brew doctor #Check your system for potential problems. Doctor exits with a non-zero status if any problems are found.
$ brew switch ansible 2.2.1.0 #切换到之前版本
$ brew edit wget # opens in $EDITOR! 编辑安装脚本
$ brew services list #查看系统通过homebrew安装的服务
$ brew services cleanup #清除已卸载无用的启动配置文件
    Removing unused plist /Users/icyleaf/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    Removing unused plist /Users/icyleaf/Library/LaunchAgents/homebrew.mxcl.redis.plist
```
> Homebrew-tap 是一个包管理器（brew tap） 存储库，包含了 Pivotal 产品上的一些 "公式（formulae）"

