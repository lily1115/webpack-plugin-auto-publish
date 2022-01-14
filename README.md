# webpack-plugin-auto-publish

将编译完成的前端文件放置发布项目中

## Getting Started

开始, 你需要安装 `webpack-plugin-auto-publish`:

```console
$ npm install webpack-plugin-auto-publish --save-dev
```


vue vue-cli-service 项目配置如下

> 由于 run dev 默认的就是 development, 与我们打包分支 dev 重复，所以要在项目中 将.env.development copy一份 命名为 .env.dev 文件。 添加 NODE_ENV = 'dev'。然后 package.json 更改 `"build:dev": "vue-cli-service build  --mode dev",`

**vue.config.js**

```js

const autoPublish = require('webpack-plugin-auto-publish');
const BRANCHS = { dev: 'dev', production: 'test' }
const BRANCH = BRANCHS[process.env.NODE_ENV || 'dev']

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

然后添加 `webpack` 配置. 如下:


**webpack.config.js**

```js

const autoPublish = require('webpack-plugin-auto-publish');

const BRANCHS = { dev: 'dev', production: 'test' }
const BRANCH = BRANCHS[process.env.NODE_ENV || 'dev']

module.exports = {
  plugins: [
    new autoPublish({
        gitLab: 'http://192.168.110.22/neoscholar/neo_vue/lesson-study-class-interaction-preview.git',
        env: BRANCH,                        //分支名称
        version: (new Date).getTime(),      //打包版本
        dir: '/publish/dist/',                //移动复制打包文件至XXX
        filter: /^.*$/                      //需要移动复制的文件
    }),
  ],
};
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