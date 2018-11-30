## Webpack Plugin Base

这是编写 webpack 插件最基本的代码

```js
'use strict';

function WebpackPlugin(options) {
  var defaultOptions = {
    onBuildStart: () => {},
    onBuildEnd: () => {}
  };

  this.options = Object.assign(defaultOptions, options);
}

WebpackPlugin.prototype.apply = function(compiler) {
  const options = this.options;

  //开始构建时执行
  compiler.plugin('compilation', compilation => {
    options.onBuildStart();
  });

  //生成资源到 output 目录之前执行
  compiler.plugin('emit', (compilation, callback) => {
    options.onBuildEnd();
    callback();
  });
};

module.exports = WebpackPlugin;
```

## 使用示例

```js
plugins: [
  new WebpackPlugin({
    onBuildStart: () => console.log('构建开始时执行'),
    onBuildEnd: () => console.log('构建结束后执行')
  })
];
```

## 常用钩子

- ###### `emit` 生成资源到 output 目录之前。
- ###### `afterEmit` 生成资源到 output 目录之后。
- ###### `done` 编译(compilation)完成。
- ###### `failed` 编译(compilation)失败。

更多钩子事件请[点我](https://webpack.js.org/api/compiler-hooks/)
