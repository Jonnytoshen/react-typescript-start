# react-typescript-start
React development environment based on typescript.

## 添加支持Less
在开始修改配置之前，先安装编译Less所依赖的loader:
```
npm install less-loader less --save-dev
```
依赖loader安装完成后，在 /config/webpack.config.js 文件的 style files regexes 区域添加下面两行代码：
```
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;
```
接下来我们继续配置编译less的loader。在file loader之前添加以下代码：
```
// Opt-in support for LESS (using .less extensions).
// By default we support LESS Modules with the
// extensions .module.less
{
    test: lessRegex,
    exclude: sassModuleRegex,
    use: getStyleLoaders(
    {
        importLoaders: 2,
        sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
    },
    'less-loader'
    ),
    // Don't consider CSS imports dead code even if the
    // containing package claims to have no side effects.
    // Remove this when webpack adds a warning or an error for this.
    // See https://github.com/webpack/webpack/issues/6571
    sideEffects: true,
},
// Adds support for CSS Modules, but using LESS
// using the extension .module.less
{
    test: lessModuleRegex,
    use: getStyleLoaders(
    {
        importLoaders: 2,
        sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
        modules: true,
        getLocalIdent: getCSSModuleLocalIdent,
    },
    'less-loader'
    ),
},
```
现在开发环境已经支持编译Less样式了，我们可以在项目中使用Less来编写样式。