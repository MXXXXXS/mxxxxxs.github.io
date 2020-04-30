# vscode调试配置launch.json

单独调试c++模块, [参考](https://stackoverflow.com/a/54344811/8481407)

```json
{
  // 使用 IntelliSense 了解相关属性。 
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "c++ addon 调试",
      "type": "cppvsdbg",
      "request": "launch",
      "program": "node",
      "args": ["hello.js"],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
    }
  ]
}
```

混合调试, 使用"attach"类型, [参考]( https://toyobayashi.github.io/2018/10/19/VSCodeMixDebug/ )

在js和c++中都打上断点, 先启动"c++ addon 调试", 再切换启动"c++ addon Attach", 选择调试进程(一个node进程 ), 就可以同时调试js和c++了.

```json
{
  // 使用 IntelliSense 了解相关属性。 
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "node 调试",
      "program": "${file}",
      "skipFiles": [
        "<node_internals>/**"
      ]
    },
    {
      "name": "c++ addon 调试",
      "type": "cppvsdbg",
      "request": "launch",
      "program": "node",
      "args": ["${file}"],  //js文件, 作为入口
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
    },
    {
      "name": "c++ addon Attach",
      "type": "cppvsdbg",
      "request": "attach",
      "processId": "${command:pickProcess}"
    }
  ]
}
```

