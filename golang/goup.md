# Golang版本管理工具goup

- 记录时间: 2020-12-07
- 版本信息: v0.1.6

[github地址](https://github.com/owenthereal/goup)

## goup安装

安装前需要注意两个环境变量: 

- `GOUP_UPDATE_ROOT`用于指定下载`goup`时的地址, 默认地址是`https://github.com/owenthereal/goup/releases/latest/download`
- `GOUP_GO_HOST`用于指定下载`go二进制文件`的主机名, 默认地址是`golang.org`

```bash
$ export GOUP_UPDATE_ROOT=https://gh.api.99988866.xyz/https://github.com/owenthereal/goup/releases/latest/download;\
export GOUP_GO_HOST=golang.google.cn;\
curl -sSf https://raw.githubusercontent.com/owenthereal/goup/master/install.sh | sh
```

> 注意: 如果`raw.githubusercontent.com`无法访问, curl: (6) Could not resolve host: raw.githubusercontent.com, 可以更换为下面地址`https://cdn.jsdelivr.net/gh/owenthereal/goup@master/install.sh`
> 默认会进行`最新版本`的go二进制安装, 可增加参数跳过`sh -s -- '--skip-prompt' '--skip-install'`

安装日志: 

```bash
Welcome to Goup!                                                                       
                                                                                       
Goup and Go will be located at:                                                        
                                                                                       
  C:\Users\Administrator\.go                                                           
                                                                                       
The Goup command will be located at:                                                   
                                                                                       
  C:\Users\Administrator\.go\bin                                                       
                                                                                       
The go, gofmt and other Go commands will be located at:                                
                                                                                       
  C:\Users\Administrator\.go\current\bin                                               
                                                                                       
To get started you need Goup's bin directory (C:\Users\Administrator\.go\bin) and      
Go's bin directory (C:\Users\Administrator\.go\current\bin) in your PATH environment   
variable. These two paths will be added to your PATH environment variable by           
modifying the profile files located at:                                                
                                                                                       
  C:\Users\Administrator\.profile                                                      
  C:\Users\Administrator\.zprofile                                                     
  C:\Users\Administrator\.bash_profile                                                 
                                                                                       
Next time you log in this will be done automatically. To configure your                
current shell run source $HOME/.go/env.                                                
                                                                                       
Would you like to proceed with the installation: y                                     
Would you like to proceed with the installation: y                                     
                                                                                       
Downloaded   0.0% (    16384 / 138876190 bytes) ...                                    
Downloaded   4.0% (  5619104 / 138876190 bytes) ...                                    
Downloaded   8.8% ( 12206032 / 138876190 bytes) ...                                    
Downloaded  14.6% ( 20283280 / 138876190 bytes) ...                                    
Downloaded  20.1% ( 27901744 / 138876190 bytes) ...                                    
Downloaded  24.2% ( 33586944 / 138876190 bytes) ...                                    
Downloaded  26.1% ( 36290288 / 138876190 bytes) ...                                    
Downloaded  32.5% ( 45202544 / 138876190 bytes) ...                                    
Downloaded  33.9% ( 47087296 / 138876190 bytes) ...                                    
Downloaded  35.1% ( 48758416 / 138876190 bytes) ...                                    
Downloaded  40.9% ( 56770128 / 138876190 bytes) ...                                    
Downloaded  45.8% ( 63667744 / 138876190 bytes) ...                                    
Downloaded  51.6% ( 71646688 / 138876190 bytes) ...                                    
Downloaded  57.6% ( 79953312 / 138876190 bytes) ...                                    
Downloaded  60.1% ( 83508752 / 138876190 bytes) ...                                    
Downloaded  64.6% ( 89733920 / 138876190 bytes) ...                                    
Downloaded  71.2% ( 98827568 / 138876190 bytes) ...                                    
Downloaded  77.5% (107592944 / 138876190 bytes) ...                                    
Downloaded  83.8% (116374672 / 138876190 bytes) ...                                    
Downloaded  87.8% (121994400 / 138876190 bytes) ...                                    
Downloaded  92.7% (128679024 / 138876190 bytes) ...                                    
Downloaded  98.1% (136198624 / 138876190 bytes) ...                                    
Downloaded 100.0% (138876190 / 138876190 bytes)                                        
INFO[0039] Unpacking C:\Users\Administrator\.go\go1.15.6\go1.15.6.windows-amd64.zip ...
INFO[0102] Success: go1.15.6 downloaded in C:\Users\Administrator\.go\go1.15.6         
INFO[0102] Default Go is set to 'go1.15.6'
```

## 环境变量

goup安装完成后, 会生成并修改几个文件:

- 自动生成一个`env`文件, `$HOME/.go/env`: `export PATH="$HOME/.go/bin:$HOME/.go/current/bin:$PATH"`
- 自动在这三个文件`.bash_profile`, `.zprofile`, `.profile`中添加一行记录, `source "$HOME/.go/env"` (三个文件如果不存在, 则会默认创建)

所以执行`source .bash_profile`即可在命令行中访问到`goup`和`go`命令

## 常见问题

- windows平台下安装的时候, 最好将命令行的当前目录切换到`$HOME/.go`目录所在的盘符下, 否则安装会卡住或缓慢