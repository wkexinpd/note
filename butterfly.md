

# butterfly

### 1.下载：

```
npm install -g cnpm --registry=https://registry.npm.taobao.org （可以解决卡住不动的）
cnpm install
```

### 2. 预备知识

### canvas：

#### 2.1canvas的基本用法

`<canvas>` 标签只有两个属性**——** width和height，canvas会初始化宽度为300像素和高度为150像素。 如果你绘制出来的图像是扭曲的, 尝试用width和height属性为`<canvas>`明确规定宽高，而不是使用CSS。

baseCanvas

```js

```

canvas

```js
//属性：
	// root             根节点
    // layout           布局支持
    // zoomable         是否可放大缩小
    // moveable         是否可移动
    // draggable        是否可拖动节点
    // linkable         是否可连接线条
    // disLinkable      是否可取消连线
    // theme            主题配置
    // global           公共配置
```



### Node:

BaseNode

```js
//继承Node
draw  //节点的渲染方法，@param {obj} data - 节点基本信息 ，@return {dom} - 返回渲染dom的根节点
focus
unFocus

/**
  * @param {obj} param - 锚点基本信息(此方法必须在节点挂载后执行才有效)
  * @param {string} param.id - 锚点id
  * @param {string} param.orientation - 锚点方向(可控制线段的进行和外出方向)
  * @param {string} param.scope - 作用域
  * @param {string} param.type - 'source' / 'target' / undefined / 'onlyConnect'。 当undefined的时候锚点既是source又是target，但不能为同是为'source'和'target'，先来先到 ; 'onlyConnect'，锚点既是source又是target，可同时存在
  * @param {string} param.dom - 可以把节点内的任意一个子dom作为自定义锚点
  */
addEndpoint //添加锚点进endpoints对象中
removeEndpoint //根据pointId删除endpoints对象中的一项
getEndpoint   //找到锚点 根据id和锚点类型找到
_init  //初始化
_moveTo // drag的时候移动的api
moveTo //移动
```

Node (节点)

```js
class Node extends EventEmit3 {
  constructor() {
    // id        节点唯一标志
    // top       坐标y
    // left      坐标x
    // group     存在于哪个节点组上
    // dom       节点的dom元素
    // draggable 该节点是否能拖动标志，可覆盖全局的
    // options   数据的透传
    // _on       节点发送事件
    // _emit     节点发送事件
    // _global   全局的配置


    // 需要优化的
    // scope           scope相同可拉进group里面
    // endpoints       endpoint对象
    // _endpointsData  真实的endpoint数据
    // _isMoving       标识是否在移动做，兼容冒泡
    super();
  }

  // 渲染节点
  draw() {}

  // 获取锚点
  getEndpoint() {}

  // 添加锚点
  addEndpoint() {}

  // 删除锚点
  removeEndpoint() {}

  // 移动节点
  moveTo() {}

  // 获取宽度
  getWidth() {}

  // 获取高度
  getHeight() {}

  // 设置该节点是否能拖动，能覆盖全局
  setDraggable() {}

  // remove的方法
  remove() {}

  // 销毁的方法
  destroy() {}

  // ********* 需要新增的api ********* 

  // focus回调
  focus() {}

  // unFocus回调
  unFocus() {}

  // 单击的回调
  click() {}

  // 双击的回调
  doubleClick() {}

  // 右键的回调
  onContextmenu() {}

  // hover的回调
  hover() {}

}
```

### Edge:

baseEdge.js

`_.get(object, path, [defaultValue])`根据 `object`对象的`path`路径获取值。 如果解析 value 是 `undefined` 会以 `defaultValue` 取代。

```js
var object = { 'a': [{ 'b': { 'c': 3 } }] };
 
_.get(object, 'a[0].b.c');
// => 3
```

`_.assign(object, [sources])`分配来源对象的可枚举属性到目标对象上。 来源对象的应用规则是从左到右，随后的下一个对象的属性会覆盖上一个对象的属性。

```js
function Foo() {
  this.a = 1;
}
 
function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assign({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'c': 3 }
```

```js
//继承edge
_init //初始化，调用draw，drawLabel，drawArrow，_addEventListener
  
draw   //自定义节点的dom
   /**
     ？document.createElementNS('http://www.w3.org/2000/svg', 'path')
      document.createElementNS与document.createElement类似，也用于创建标签节点，只是它需要一个额外的命名空间URI作为参数
      	有效的命名空间URI
        HTML： http://www.w3.org/1999/xhtml
        SVG：http://www.w3.org/2000/svg
        XBL：http://www.mozilla.org/xbl
        XUL：http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul
        在svg中插入元素时，要使用document.createElementNS和setAttributeNS。即使使用setAttribute是生效的。
        
     设置 isExpandWidth 为 true 时，获取 eventHandlerDom 用于挂载事件
   */

mounted   // 线条挂载后的回调，函数中调用addAnimate
_calcPath //计算线条path：sourcePoint，targetPoint 根据这两个点绘制线条
redrawLabel  //重新计算label
drawLabel  //自定义label的dom
updateLabel  //更新label的dom  @param {string|dom} - label的字符窜或者节点
redrawArrow  //重新描绘箭头
drawArrow  // 自定义箭头的dom
redraw //重新描绘 会重新计算线条path（_calcPath） 重新计算label（redrawLabel） 重新计算arrow（redrawArrow） 重新计算动画animate（redrawAnimate）然后会调用updated（线条重绘后的回调）
isConnect

/*
   @param {obj} options(可选参数) - 配置动画效果
   @param {number} options.radius (可选参数) - 动画节点半径
   @param {string} options.color (可选参数) - 动画节点颜色
   @param {string} options.dur (可选参数) - 动画运行时间，如：1s
   @param {number|string} options.repeatCount (可选参数) - 动画重复次数，如：1 或者 'indefinite'
*/
addAnimate  // 开启动画
  addAnimate(options) {
    this.animateDom = LinkAnimateUtil.addAnimate(this.dom, this._path, _.assign({},{
      num: 1, // 现在只支持1个点点
      radius: 3,
      color: '#776ef3'
    }, options), this.animateDom);
  }

redrawAnimate
emit  //发送事件
on    //接受事件
remove
setZIndex  //设置线段z-index
destroy 
_addEventListener
_create //会调用redraw
```

edge.js

```js
class Edge extends EventEmit3 {
  constructor() {
    // id                 节点唯一标志
    // targetNode         目标节点
    // targetEndpoint     目标锚点
    // sourceNode         源节点
    // sourceEndpoint     源锚点
    // type               "node" or "endpoint"，标志类型的
    // shapeType          线条类型
    // label              设置线条上的字体
    // options            数据的透传
    // dom                线条dom
    // labelDom           label的dom
    // arrowDom           箭头的dom
      
    // 箭头部分看看需要优化不？
    // arrow              是否有箭头
    // arrowPosition      箭头的位置
    // arrowOffset        箭头起始点

    // 需要优化的
    // isExpandWidth      拓展线条
    // eventHandlerDom    代替绑定事件的dom
    // orientationLimit   限制方向

    // 需要优化的
    // scope           scope相同可拉进group里面
    // endpoints       endpoint对象
    // _endpointsData  真实的endpoint数据

    // 需要悬空
    super();
  }

  // 渲染节点
  draw() {}

  // 重绘节点
  redraw() {}

  // 渲染label
  drawLabel() {}

  // 重绘label
  redrawLabel() {}

  // 渲染arrow
  drawArrow() {}

  // 重回arrow
  redrawArrow() {}

  // 判断是否能连接的方法
  isConnect() {}

  // 销毁节点
  destroy() {}

  // ********* 需要新增的api ********* 

  // 删除线条
  remove() {}

  // 单击的回调
  click() {}

  // hover的回调
  hover() {}

  // focus回调
  focus() {}

  // unFocus回调
  unFocus() {}

}
```

### Endpoint

用法：

```js
// 用法一:
canvas.draw({
  nodes: [{
    ...
    endpoints: [{
      id: 'point_1',
      type: 'target',
      orientation: [-1, 0],
      pos: [0, 0.5]
    }]
  }]
})

// 用法二:
let node = this.canvas.getNode('xxx');
node.addEndpoint({
  id: 'xxxx',
  type: 'target',
  dom: dom           // 使用此属性用户可以使用任意的一个dom作为一个锚点
});
```

baseEndpoint

```js
//属性：
id:节点唯一标识
orientation：方向
pos:位置
type：目标锚点还是源锚点 'source' / 'target' / undefined / 'onlyConnect'，当undefined的时候锚点既是source又是             target；当onlyConnect的时候锚点既是source又是target，但锚点不能拖动删除线段
root: 可把锚点附属于某个子元素
scope：作用域 锚点scope相同才可以连线
expandArea：可以设置锚点连接的热区，可覆盖主题内的设置
limitNum：连线数目限制
connectedNum：已连接数
dom: 可以把此dom作为自定义锚点
disLinkable:禁止锚点拖动断开线段
Class:扩展类
_isInitedDom：是否自定义锚点 true/false
```

```js
//API
_init 
draw 自定义节点的dom
updatePos 如果有自定义锚点，则计算width,height,left,top;否则分情况弄好方向和位置，计算节点本身的偏移量
moveTo 
linkable 设置连线linkable的状态
unLinkable 取消连线时linkable的状态
hoverLinkable 设置连线时linkable并且hover状态 
unHoverLinkable 取消连线时linkable并且hover状态
attachEvent 
emit
destroy
```

endpoint

```js
class Endpoint extends EventEmit3 {
  constructor() {
    super();
  }
}

export default Endpoint;
```

### 3.example

#### 3.1 analysis

##### index.jsx

画布属性：

```js
this.canvas = new Canvas({
      root: root,
      disLinkable: true, // 可删除连线
      linkable: true,    // 可连线
      draggable: true,   // 可拖动
      zoomable: true,    // 可放大
      moveable: true,    // 可平移
      theme: {			 //主题定制
        edge: {
          type: 'Straight', //线条默认类型：贝塞尔曲线，折线，直线，曼哈顿路由线，更美丽的贝塞尔曲线。分别为Bezier/Flow/Straight/Manhattan/AdvancedBezier  
        }
      }
  });
```

API：

```js
/*
渲染方法：
mockData:data.js 包含分组，节点，连线
callback 回调函数（渲染过程是异步）
*/
this.canvas.draw(mockData, () => {

});
this.canvas.on('events', (data) => {
    console.log(data);
});
```

##### data.js

```js
 nodes: [
    {
      id: '1',  //节点唯一标识
      label: 'common..', //显示的内容
      iconType: 'icon-rds', //节点类型，例如圆形，矩形  icon-rds/icon-guize-kai/circle
      top: 25,  //y轴坐标
      left: 0,  //x轴坐标
      Class: Node, //拓展类 当传入拓展类的时候，该节点组则会按拓展类的draw方法进行渲染，拓展类的相关方法也会覆盖父类的方法
      endpoints: [{ //锚点信息 小圆点
        id: 'bottom', //在该节点的下方
        orientation: [0, 1], //方向：下:[0,1]/上:[0,-1]/右:[1,0]/左:[-1,0],除了控制系统锚点方向，而且能控制线段的出入口方向
        pos: [0.5, 0] //位置
      }]
    },
  ]
edges: [{ //线
      source: 'bottom', //连接源锚点id
      target: 'top', //连接目标锚点id
      sourceNode: '1', //连接源节点id
      targetNode: '4', //连接目标节点id
      arrow: true,  //线条上加箭头
      type: 'endpoint',//标志线条连接到节点还是连接到锚点
      arrowPosition: 0.9, //箭头在线条上的位置
      Class: Edge // 拓展类
    },
 ]
```

##### edge.js

```js
//继承baseEdge
draw()  return path
drawArrow()  return dom;
drawLabel()  return dom;
```

##### node.js

```js
//继承BaseNode
class BaseNode extends Node {
  constructor(opts) {
    super(opts);
    this.options = opts;
  }
  draw = (opts) => {
    if (opts.options.type === 'circle') {
      const container = $('<div class="analysis-circle-base-node"></div>')
                      .attr('id', opts.id)
                      .css('top', opts.top + 'px')
                      .css('left', opts.left + 'px');
      const icon = $(`<i class="iconfont icon-shujuji"></i>`);

      container.append(icon);           
      
      return container[0];            
    }
    const container = $('<div class="analysis-base-node"></div>')
                    .attr('id', opts.id)
                    .css('top', opts.top + 'px')
                    .css('left', opts.left + 'px')

    // this._createTypeIcon(container);
    this._createText(container);

    return container[0];
  }
  _createTypeIcon(dom = this.dom) {
    const iconContainer = $(`<span class="icon-box ${this.options.className}"></span>`)[0];
    const icon = $(`<i class="iconfont ${this.options.iconType}"></i>`)[0];

    iconContainer.append(icon);
    $(dom).append(iconContainer);
  }
  _createText(dom = this.dom) {
    $('<span class="text-box"></span>').text(this.options.label).appendTo(dom);
  }
}
```

