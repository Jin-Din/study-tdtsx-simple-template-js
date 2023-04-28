<template>
  <div class="mapcontainer" id="mapcontainer"></div>
</template>

<script setup>
import { onMounted } from "vue";
const { mapboxgl } = window; //结构获得mapboxgl

mapboxgl.accessToken = "pk.eyJ1Ijoib25lZ2lzZXIiLCJhIjoiY2plZHptcnVuMW5tazMzcWVteHM2aGFsZiJ9.ERWP7zZ-N6fmNl3cRocJ1g";

onMounted(() => {
  const map = new mapboxgl.Map({
    container: "mapcontainer", //地图容器
    style: "https://shaanxi.tianditu.gov.cn/ServiceSystem/Tile/rest/service/sxww2022Geo/token/VectorTileServer/styles/default.json", //地图地址
    center: [108.95622, 35.279541], //中心点位置
    minZoom: 6,
    zoom: 6, //地图缩放比例
    // pitch: 45, //倾斜角度
  });

  map.on("load", () => {
    //地图初始化完成后做的事情
    // do something
    //比如：加一个五角星
    let base64Url = `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAAXNSR0IArs4c6QAAAsZJREFUWEfFl09IFGEYxp93WkH2e2ehLkGQB1cJAuvSoUtQEITSUagOFRUFgnqpQ3+MlP5gUBetS4FQXYq6ldpBKMJDlyDxEKHrIerQRcLvnUXKnTd20bB1ZnZmVmsuA/s97/P8eL95v5kl/OeL6skXYwaVKOuK9Kb1SQ0grnsKqiPlYAW6XZH7aSBSA1jmdwTsWw79xCI7/xmAZe4k4PnqQJ/oUs7awaQQqTogzB8B7K4K+8oi2zccQIw5BKLXgUFEF9na20kgEndAmL8B2BYS8oNFNm8YgJfNtqvjjEUF+ERnctZWpiPOlagDwvwTQEMN4xKLZOKElzWBAPONjU0NmUxeifKbfL+5fCegQwGOaTwPYIJUCyXHmSvffy0tFbYsLn6prq8AWOZ+Imom1bwCeQBbYwYllX0noKBEBVWdc0X6qRIOXEvqFKanpiY4bW0ojY7WtFRggDzXPayqL2uqEwgaurpQmpyEPz0dWaXAscoWeK57WVVvJsioW0pEV4y1t/48hGLMeRDdqds5joHqBfa8u2umoMjc7QPDcTzSahygJytyb6V+zRh6rntWVR+kDYiqI6JzxtqHqzWB54AYcwJEj9YVQvUke97jwHMgKKjIfMQHnq4HhAMczYo8C/KKPIqFeQrArjohPrDIntBzI8rcMs8Q0FIPgAKzrkhrKgBh1nrCV2pZJLTToQuLuVzLku/PrAdAxnFaGxcWZhM9A3He/XHhyPc7TLE4ngigyNzjA0NxQ6J0DtCbFQk84EK3wDIPEdBTA+Dt8vr+KJ0Cw2F/XkIBxHXHoNoe0rb3qnqVPW+ivC7GHCSi6wrsDQQhGmdrOxJtQcgIThFRn7H2VZDZ8qv9RvUne9Qohnfg7xH87AB9WZEXcZ6JInOnD5RBdtQaxSiAN5Vi1RH2vCdxgqs1YsxxEJ0u/84iBxJtQZrANDW/AVky+A/dA0+/AAAAAElFTkSuQmCC`;
    map.loadImage(base64Url, (error, simg) => {
      map.addImage("marker-red", simg);
    });

    map.addSource("center_source", {
      type: "geojson",
      data: {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [108.949, 34.268],
        },
        properties: {
          name: "陕西省",
          color: "red",
        },
      },
    });

    map.addLayer({
      id: "center_text_layer",
      type: "symbol",
      source: "center_source",
      layout: {
        "icon-image": "marker-red",
        "icon-size": 0.6,
        "icon-offset": [0, 0],
        "text-field": "{name}",
        "text-offset": [0, -1],
        "text-size": 18,
      },
      paint: {
        "text-color": ["get", "color"], //使用配置的颜色
        "text-halo-color": "#ffffff",
        "text-halo-width": 2,
      },
    });
  });
});
</script>

<style scoped>
.mapcontainer {
  width: 100%;
  height: 100%;
}
</style>
