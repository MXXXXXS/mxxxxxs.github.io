# webpack-dev-server配置

这里指对渲染进程的配置, 主进程使用webpack的`watch`就可以了

1. 主进程`loadURL`使用http协议(由webpack-dev-server配置指定)加载, 而不是file协议(打包成独立应用时再改回file). (开发阶段如此, 配合webpack-dev-server)
2. 使用`HtmlWebpackPlugin`时注意设置`output.publicPath`, `HtmlWebpackPlugin`依据此给加入的文件添加路径前缀

