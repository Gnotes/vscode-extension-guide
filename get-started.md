# 新手引导

[官方](https://code.visualstudio.com/api) 介绍通过 `VS Code Extension Generator` 生成初始化项目

```bash
npm install -g yo generator-code

yo code
```

### 项目结构

```bash
.
├── .vscode
│   ├── launch.json     # 启动和调试配置
│   └── tasks.json      # TypeScript 编译任务配置
├── src                 # 源码目录
│   └── extension.ts    # 入口文件
├── package.json        # 清单文件 ⚠️重要！！
├── tsconfig.json       # TypeScript 配置
```

- launch.json : [Debugging](https://code.visualstudio.com/docs/editor/debugging)
- tasks.json : [Tasks](https://code.visualstudio.com/docs/editor/tasks)
- tsconfig.json : [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### 运行插件

键入 **_F5_** 会编译项目并打开新的 `开发模式` 窗口，通过命令面板 **`Cmd + Shift + P`** ，搜索 `hello world` 命令并执行.

### 编辑插件

修改文件 `src/extension.ts` 后，重新加载窗口 **`Cmd + R`** 即可更新

### 调试插件

在 `Debug` 模式下可以直接进行调试

### 重要概念 ⚠️

- [Activation Events](https://code.visualstudio.com/api/references/activation-events) ( eg. `onCommand`)
  > 激活事件，用于 **匹配** 用户或编辑器的操作，并触发对应处理函数或操作
- [Contribution Points](https://code.visualstudio.com/api/references/contribution-points) ( eg. `contributes.commands`)
  > 在 `package.json` 中 **声明** 命令插件提供的功能(贡献)
- [VS Code API](https://code.visualstudio.com/api/references/vscode-api) ( eg. `commands.registerCommand`)
  > 底层 JS-API

### 清单文件

插件配置都写在 `package.json` 中，详细配置查看 [Extension Manifest](https://code.visualstudio.com/api/references/extension-manifest) 或 [VSCode 插件开发全攻略 - package.json 详解](https://www.cnblogs.com/liuxianan/p/vscode-plugin-package-json.html)

### 入口文件

此文件抛出了两个函数 `activate`、 `deactivate`，都会被自动调用，**名字是不能改的**，且 activate 是必须的，deactivate 非必须.

```js
import * as vscode from 'vscode';

// 插件激活时调用，默认是没有激活的
export function activate(context: vscode.ExtensionContext) {
  // 调用 VS Code API 注册事件(Activation Events)监听函数
  let disposable = vscode.commands.registerCommand('extension.helloWorld', () => {
    vscode.window.showInformationMessage('Hello World!');
  });

  context.subscriptions.push(disposable);
}

// 插件进入“停用?”状态前自动执行的函数
export function deactivate() {}
```

解释：

> **插件** 在 Contribution Points （`contributes.commands`） 声明了事件，当触发事件 Activation Events(`onCommand`) 时，就会执行我们在 VS Code API(`vscode.commands.registerCommand`) 注册的监听函数

### 示例仓库

更多示例参考：[vscode-extension-samples](https://github.com/Microsoft/vscode-extension-samples)
