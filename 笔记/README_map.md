
// lng lat 坐标系从左下角开始算 0, 0    与js左上角 0, 0不同
// 意味着js如果需要往下放，它的纵坐标越大
// 而地图如果需要往下放，则纵坐标越小
// 高德地图的区域canvas坐标顺序是   左上  右上  右下  左下


// const polygonArr = [
//   [Number(item.longitude - 0.05), Number(item.latitude + 0.05)],
//   [Number(item.longitude + 0.05), Number(item.latitude + 0.05)],
//   [Number(item.longitude + 0.05), Number(item.latitude - 0.05)],
//   [Number(item.longitude - 0.05), Number(item.latitude - 0.05)]
// ]

// const obj = {
//   map: this.map,
//   path: polygonArr,//设置多边形边界路径
//   strokeColor: "transparent", //线颜色
//   fillColor: "black", //填充色
// }

// var polygon = new AMap.Polygon(obj);




## 问题：目前的地图区域填充图，地图不会显示点击到当前区域的下属区域

意思就是：我比如选择选过范围，并不会展现所有的省的名字
如果我点击某个省份，地图展开这个省份的地图后：并不会展现各个城市的名字
这样就给点击地图造成了很大的麻烦，我想直接点击某个地方，还不知道是哪片区域。



解决方案：使用文本标记 配合点击某个区域的时候，获取当前节点的下属节点

这两个配合，在点击的时候，获取当前节点的所有下属节点
遍历给每个城市的center坐标加上一个透明底色的文本标记

下次再打标记的时候，要记得清除之前的标记
（这个要配合删除自定义地图的所有文字显示，不然到处都是文字）






















重点1： 地图点击重新绘图并拿到对应的adcode去后台查询点位


点击当前区域之外会触发 outsideClick 效果，

1、如果是当前省份的地图，会获得省份的数据
2、如果点击的是其他省份，则获得对应那个省份的数据
3、点到了外国，获得中国的adcode

点击树结构会直接调用 重新渲染的方法，能够直接获取到adcode

点击当前地图下探也可以拿到adcode


这三个点击效果最终都会走到 refreshAreaNode() 里面
而且是有变化效果才会走到这里面

所以可以在 refreshAreaNode() 里面拿到变化的那个adcode
然后到后台获取打点数据，清空当前打点，并重新打点





重点2：打点位置上面附加点位数据，悬浮效果时，获取打点数据，进行展示

在打点数据的对象上的 extData 设置对象

然后在  e.target.getExtData() 获取自己设置的对象








