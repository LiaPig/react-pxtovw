# React umi使用postcss将px转为vw来自适应

## 一、通过umi来搭建app（如果已新建项目请跳过此步骤）

[通过脚手架create-umi创建项目](https://umijs.org/zh/guide/create-umi-app.html#%E4%BB%8B%E7%BB%8D-create-umi)

1. 创建目录，初始化umi

```
mkdir react-pxtovw && cd react-pxtovw
yarn create umi
```
![创建的时候选择app就足够了，然后也使用typescript](https://upload-images.jianshu.io/upload_images/7016617-6a2fe05b1c487950.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2. 安装依赖
```
yarn
```


## 二、使用vw来实现自适应

参考：[如何在Vue项目中使用vw实现移动端适配](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)

1. 在`html`文件的`<head></head>`标签内插入以下代码：

```
<meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no,width=device-width">
```

> PS: 如果没有找到html文件，需要新建 [src/pages/document.ejs](https://umijs.org/zh/guide/html-template.html)


2.安装依赖

```shell
cnpm install -D postcss-import postcss-url cssnano-preset-advanced 
cnpm install -S postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext cssnano postcss-viewport-units
```

3.引入和配置

打开且修改.umirc.ts`文件：

```
// 引入步骤2安装的依赖们
import postcssImport from 'postcss-import';
import postcssUrl from 'postcss-url';
import postcssAspectRatioMini from 'postcss-aspect-ratio-mini';
import postcssWriteSvg from 'postcss-write-svg';
import postcsscssnext from 'postcss-cssnext';
import pxToViewPort from 'postcss-px-to-viewport';
import cssnano from 'cssnano';

const config: IConfig =  {
  // 这里省略了很多...

  // 额外的postcss插件用这个extraPostCSSPlugins：
  extraPostCSSPlugins: [
    postcssImport({}),
    postcssUrl({}),
    postcssAspectRatioMini({}),
    postcssWriteSvg({ utf8: false }),
    postcsscssnext({}),
    pxToViewPort({
      viewportWidth: 720, // (Number) The width of the viewport.
      viewportHeight: 1280, // (Number) The height of the viewport.
      unitPrecision: 3, // (Number) The decimal numbers to allow the REM units to grow to.
      viewportUnit: 'vw', // (String) Expected units.
      selectorBlackList: ['.ignore', '.hairlines'], // (Array) The selectors to ignore and leave as px.
      minPixelValue: 1, // (Number) Set the minimum pixel value to replace.
      mediaQuery: false // (Boolean) Allow px to be converted in media queries.
    }),
    cssnano({
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false,
      zindex: false
    })
  ],
}
```


> PS：用到的 [extrapostcssplugins](https://umijs.org/zh/config/#extrapostcssplugins) 。—— 定义额外的 PostCSS 插件，格式为数组。



## 三、show me the code

写个div，目录地址为：`react-pxtovw/src/pages/index.tsx`：

```
<div className={styles.test}>测试</div>
```

再编写下css样式，目录地址为：`react-pxtovw/src/pages/index.css`：

```
.test {
  display: inline-block;
  width: 500px;
  height: 500px;
  line-height: 500px;
  text-align: center;
  color: #fff;
  background: pink;
}
```



![页面展示](https://upload-images.jianshu.io/upload_images/7016617-501cfc680bc8f03e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![写的是px，编译后自动转为vw](https://upload-images.jianshu.io/upload_images/7016617-a80b6b15a768155f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
