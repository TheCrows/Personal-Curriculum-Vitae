# webpack

## loader

### css-loader style-loader postcss-loader less-loader sass-loader node-sass dart-sass

file-loader解决图片引入问题，并将图片 copy 到指定目录，默认为 dist

url-loader解依赖 file-loader，当图片小于 limit 值的时候，会将图片转为 base64 编码，大于 limit 值的时候依然是使用 file-loader 进行拷贝

img-loader压缩图片

babel-loader 使用 Babel 加载 ES2015+ 代码并将其转换为 ES5

#### webpack5 新增资源模块(asset module)，允许使用资源文件（字体，图标等）而无需配置额外的 loader。

## plugin

### mini-css-extract-plugin（通过 CSS 文件的形式引入到页面上）

## treeshaking

## bundle

## 热更新原理

## vite

