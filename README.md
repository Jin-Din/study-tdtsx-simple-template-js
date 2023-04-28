> 本文档简要介绍了如何使用 vue 工程引入陕西省地理信息公共服务平台的地图服务进行开发。

## 一、技术栈及相关开发资源

- [vue3](https://cn.vuejs.org/) +[vite](https://cn.vitejs.dev/) + [mapbox-gl](https://docs.mapbox.com/mapbox-gl-js/api/) +js
- [陕西省地理信息公共服务平台开发帮助](https://shaanxi.tianditu.gov.cn/portal/#/devDocument)

> 关于 vue3、vite、mapboxgl 的知识，详见相关的帮助文档，本文不在阐述

## 二、创建 VUE 工程

使用 vite 命令 创建工程

```
npm create vite@latest
或
yarn create vite
或
pnpm create vite

```

如何创建 vite 工程，请参阅[搭建第一个 Vite 项目](https://cn.vitejs.dev/guide/)

创建完成后，使用 npm install 初始化项目

```
npm install
或
yarn install
或
pnpm install
```

## 三、引入地图开发包

### 1、地图开发包资源

陕西省地理信息公共服务平台提供了专门的地图 API 包进行访问地图服务。地图 API 包提供了 CGCS2000 和 web 墨卡托两种地图坐标系的 api，用户可根据需要选择调用不同坐标系的地图并选择相应的 API。

- [js 文件（cgcs2000）](https://shaanxi.tianditu.gov.cn/vectormap/YouMapServer/JavaScriptLib/mapbox-gl-cgcs2000.js)
- [js 文件（web 墨卡托）](https://shaanxi.tianditu.gov.cn/vectormap/YouMapServer/JavaScriptLib/mapbox-gl.js)
- [css 文件](https://shaanxi.tianditu.gov.cn/vectormap/YouMapServer/JavaScriptLib/mapbox-gl.css)

> 地图 API 包基于 mapboxgl 基础上扩展封装而成，功能上与原生的 mapboxgl 没有差别。详细使用请参阅[mapbox-gl 开发帮助文档](https://docs.mapbox.com/mapbox-gl-js/api/)

### 2、引入方式

打开 index.html， 文件内直接引入以上提供的 js、css 地址

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + Vue</title>
    <script src="https://shaanxi.tianditu.gov.cn/vectormap/YouMapServer/JavaScriptLib/mapbox-gl-cgcs2000.js"></script>
    <link rel="stylesheet" href="https://shaanxi.tianditu.gov.cn/vectormap/YouMapServer/JavaScriptLib/mapbox-gl.css" />
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

### 3、使用地图对象

在 vue 代码中需要使用的地方使用 widow 解构，比如：

```javascript
//获得 mapboxgl
const { mapboxgl } = window; //结构获得mapboxgl
mapboxgl.accessToken = "你自己申请的key";
//初始化创建map对象
const map = new mapboxgl.Map(option);
```

以上代码为举例，详细参见下文中的使用。

## 四、在项目工程中使用

地图包引入后，在工程中进行如下的简单配置。

### 1.样式重置 style.css

打开 style.css 文件，清空并复制以下代码:

```
* {
  margin: 0;
  padding: 0;
}

html,
body {
  width: 100%;
  height: 100%;
}

#app {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

/* 隐藏logo */
.mapboxgl-ctrl-logo {
  display: none !important;
}

```

### 2.App 页面

打开 App.vue 页面，清空并复制以下代码:

```
<template>
  <div class="mapcontainer" id="mapcontainer"></div>
</template>

<script setup>
import { onMounted } from "vue";
const { mapboxgl } = window; //结构获得mapboxgl

mapboxgl.accessToken = "你自己申请的key";

onMounted(() => {
  const map = new mapboxgl.Map({
    container: "mapcontainer", //地图容器
    //地图地址。请访问陕西省地理信息公共服务平台网站进行申请(https://shaanxi.tianditu.gov.cn/portal/#/resource)
    style: "https://shaanxi.tianditu.gov.cn/ServiceSystem/Tile/rest/service/sxww2022Geo/xxxxx/VectorTileServer/styles/default.json",
    center: [108.95, 35.27], //中心点位置
    minZoom: 6,
    zoom: 6, //地图缩放比例
    // pitch: 45, //倾斜角度
  });

  map.on("load", () => {
    //地图初始化完成后做的事情
    // do something

  });
});
</script>

<style scoped>
.mapcontainer {
  width: 100%;
  height: 100%;
}
</style>

```

### 3.运行

运行以下命令将生成访问地址，点击

```
npm run dev
```

```
 VITE v4.3.3  ready in 1066 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://10.61.128.56:5173/
  ➜  Network: http://192.168.242.1:5173/
  ➜  Network: http://192.168.25.1:5173/
```

致此，一个简单的调用陕西省地理信息公共服务平台地图服务的 vue 工程就创建完成。
