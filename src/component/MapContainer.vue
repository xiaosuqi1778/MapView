<script setup>
import { onMounted, onUnmounted } from "vue";
import AMapLoader from "@amap/amap-jsapi-loader";// 引入 JS API Loader 

let map = null;

onMounted(() => {
    window._AMapSecurityConfig = {
    securityJsCode:"0159cea799e75333853ad41b431413e5",
  }
    AMapLoader.load({
        key: "638964ba011ff16f788694feb76aaa9f", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15

        plugins: ['AMap.ToolBar', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.AutoComplete'],
        // 需要使用的的插件列表，如比例尺'AMap.Scale'等
    })
        .then((AMap) => {
            map = new AMap.Map("container", {
                // 设置地图容器id
                viewMode: "2D", // 是否为3D地图模式
                zoom: 11, // 初始化地图级别
                center: [114.31, 30.52], // 初始化地图中心点位置：武汉市
            });


            // 异步加载插件
            AMap.plugin(['AMap.ToolBar', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.AutoComplete'], function () {
                // 基本控件
                var toolbar = new AMap.ToolBar({
                    offset: AMap.Pixel(100, 100),
                    position: 'RT'
                });
                map.addControl(toolbar);
                var scale = new AMap.Scale();
                map.addControl(scale);
                // console.log(111);
                //输入提示
                var autoOptions = {
                    input: "tipinput"
                };
                //输入提示
                var placeSearch = new AMap.PlaceSearch({
                    city: 'wuhan',
                    map: map
                });  //构造地点查询类

                // js api 2.0 部分方法废弃 参见：https://lbs.amap.com/api/javascript-api-v2/update
                var auto = new AMap.AutoComplete(autoOptions);

                auto.on("select", select);//注册监听，当选中某条记录时会触发
                function select(e) {
                    placeSearch.setCity(e.poi.adcode);
                    placeSearch.search(e.poi.name);  //关键字查询查询
                }
                
            });
        })
        .catch((e) => {
            console.log(e);
        });
});

onUnmounted(() => {
    map?.destroy();
});
</script>

<!-- 创建地图容器 -->
<template>
    <div id="container" ref="container"></div>
    <div id="myPageTop">
        <table>
            <tr>
                <td>
                    <label>请输入POI关键字：</label>
                </td>
            </tr>
            <tr>
                <td>
                    <input id="tipinput" />
                </td>
            </tr>
        </table>
    </div>
</template>

<!-- 设置地图容器样式 -->
<style scoped>
html,
body {
    margin: 0;
    height: 100%;
    width: 100%;
    position: absolute;
}

#container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

#tip {
    background-color: #fff;
    padding-left: 10px;
    padding-right: 10px;
    position: absolute;
    font-size: 12px;
    right: 10px;
    top: 20px;
    border-radius: 3px;
    border: 1px solid #ccc;
    line-height: 30px;
}

#myPageTop {
    position: absolute;
    /* padding-left: 10px; */
    top: 5px;
    left: 10px;
    /* width: 220px; */
    background: #fff none repeat scroll 0 0;
    border: 2px solid #ccc;
    margin: 12px auto;
    padding: 10px;
    font-family: "Microsoft Yahei", "微软雅黑", "Pinghei";
    font-size: 16px;
}

#myPageTop label {
    margin: 0 20px 0 0;
    color: #666666;
    font-weight: normal;
}

#myPageTop input {
    width: 170px;
    height: 24px;
}

#myPageTop .column2 {
    padding-left: 25px;
}
</style>



