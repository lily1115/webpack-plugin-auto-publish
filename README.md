# webpack-plugin-auto-publish

将编译完成的前端文件放置发布项目中

## Getting Started

开始, 你需要安装 `webpack-plugin-auto-publish`:

```console
$ npm install webpack-plugin-auto-publish --save-dev
```

然后添加 `webpack` 配置. 如下:

**webpack.config.js**

```js

const jpushPublish = require('webpack-plugin-auto-publish');

const BRANCHS = { development: 'dev', production: 'test' }
const BRANCH = BRANCHS[process.env.NODE_ENV || 'development']

module.exports = {
  plugins: [
    new jpushPublish({
        gitLab: 'http://192.168.110.22/neoscholar/neo_vue/lesson-study-class-interaction-preview.git',
        env: BRANCH,                        //分支名称
        version: (new Date).getTime(),      //打包版本
        dir: '/publish/dist/',                //移动复制打包文件至XXX
        filter: /^.*$/                      //需要移动复制的文件
    }),
  ],
};
```

**webpack.config.js**
```js

configureWebpack: (config) => {
  config.plugins.push(
    new PublishPlugin({
      gitLab: 'http://192.168.110.22/neoscholar/neo_vue/lesson-study-class-interaction-preview.git',
      env: BRANCH,
      version: new Date().getTime(),
      dir: '/publish/dist/',
      filter: /^.*$/,
    })
  )
}
```


> ℹ️ `webpack-plugin-auto-publish` 插件主要用于前端代码打包后复制搬运到发布项目目录.

### Patterns

|               Name                |        Type         |                     Default                     | Description                                                                                           |
| :-------------------------------: | :-----------------: | :---------------------------------------------: | :---------------------------------------------------------------------------------------------------- |
|          [`gitLab`](#gitLab)      |     `{String}`      |  `undefined`                                    | 发布部署项目的仓库地址                                                                                 |
|            [`env`](#env)          |     `{String}`      |            `compiler.options.output`            | 发布环境-对应发布项目分支.                                                                              |
|       [`version`](#context)       |     `{String}`      | `options.context \|\| compiler.options.context` | 打包版本                                                                                              |
|        [`dir`](#totype)           |     `{String}`      |                   `undefined`                   | 发布项目存放编译文件目录XXX                                                                             |
|          [`filter`](#test)        |     `{RegExp}`      |                   `undefined`                   | 需要移动复制至发布项目里的文件                                                                          |

## License

[MIT](./LICENSE)