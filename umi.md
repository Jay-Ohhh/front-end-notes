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

