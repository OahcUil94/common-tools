# nvm版本管理工具

## nvm安装

```bash
curl -o- https://cdn.jsdelivr.net/gh/nvm-sh/nvm@v0.37.2/install.sh | bash
# 配置node的国内镜像
echo "export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node" >> ~/.bashrc
source .bashrc
```

## nvm安装node

```bash
nvm install v14.15.3 --default
```

# 配置nvm install的国内镜像地址

```bash
npm config -g set registry https://registry.npm.taobao.org
npm config -g get registry
```
