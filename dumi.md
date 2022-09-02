### doc 模式

- docs文件夹下的index.md均为doc、site模式的首页。

- 与site模式不同，src下的.md文件不会生成侧边菜单，需要在docs文件夹下新建文件夹和文件生成路由，若需分组则需新建文件夹。



### package.json 配置

```json
{
  "description": "A elegant React toast",
  "keywords": [
    "elegant-toast",
    "react",
    "totast",
    "message",
    "notification"
  ],
  "author": {
    "name": "jay_ohhh",
    "email": "1598353326@qq.com"
  },
  "homepage": "https://github.com/Jay-Ohhh/elegant-toast",
  "repository": {
    "type": "git",
    "url": "https://github.com/Jay-Ohhh/elegant-toast.git"
  },
  "bugs": {
    "url": "https://github.com/Jay-Ohhh/elegant-toast/issues"
  },
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "typings": "dist/index.d.ts",
  "files": [
    "dist",
    "package.json",
    "LICENSE",
    "README.md"
  ],
  "license": "MIT",
  "gitHooks": {
    "pre-commit": "lint-staged"
  },
  "lint-staged": {
    "src/**": [
      "npm run lint"
    ]
  },
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "react-dom": "^16.8.0 || ^17.0.0 || ^18.0.0"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "Firefox ESR",
    "not dead",
    "IE 11",
    "not IE 10"
  ]
}
```





### 常见问题

#### 配置babel模式打包，在代码内使用@/引入的，会报找不到模块的错

1、用rollup模式打包

2、

```sh
npm i eslint-import-resolver-babel-plugin-root-import babel-plugin-import babel-plugin-root-import —save-dev
```

在fatherrc.ts配置

```js
extraBabelPlugins: [
    [
      'babel-plugin-root-import',
      {
        rootPathSuffix: 'src/‘,
        rootPathPrefix: ‘@/‘,
      },
    ],
  ],
```

在eslint配置

```js
settings: {
 
    'import/resolver': {
 
      'babel-plugin-root-import': {
 
        rootPathSuffix: 'src/‘,
        rootPathPrefix: ‘@/‘,
      },
    },
  },
```

