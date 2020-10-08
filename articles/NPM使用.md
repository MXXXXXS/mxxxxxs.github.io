## 查看已全局安装装的包

`npm list -g --depth 0`

## 方便的换源方式

使用`nrm`工具

查看已有的源

`nrm ls`

换源

`nrm use taobao`

# 解决一堆镜像源的繁琐配置

使用 [mirror-config-china](https://www.npmjs.com/package/mirror-config-china), 安装后会将一堆镜像源写入~/.npmrc, 不用再手动维护