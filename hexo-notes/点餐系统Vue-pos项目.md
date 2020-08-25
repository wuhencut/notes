## 自己做个小项目练练手
<a href="https://github.com/wuhencut/vue-pos">源码</a>
这个小项目的搭建可系统的回顾与加深vue项目搭建所需要的知识点。麻雀虽小五脏俱全

### 这个项目用到的技术栈
- Vue
- vue-router
- vue-cli
- axios 
- ElementUI

### 学到的知识
- el-row 一行
- el-col一列，可类似于bootstrap设置占据的宽度，:span="7";
- 了解el-table如何遍历，如何绑定遍历数据，如何传入当前对象对参数
- el-table-column的templat必须加 slot-scope = "scope"不然不显示内部， 当前对象作为参数： scope.row
- el-button el的按钮，有自己的样式，success,warning,danger等等
- el-tabs的使用，她其实就是一个分页的标签，有点类似ul
- el-tab-pane 单页，类似于li

### axios
#### 引入axios
```javascript
npm install axios --save
```
在组件中引入axios
```javascript
import axios from 'axios'
```
axios的then方法中如果使用function，this为undefined，不知道为啥，用箭头函数，this指向外部vue实例相当于用function函数在外面声明了let t = this;


### 数组filter
> 可根据条件删选元素成为新的数组
```javascript
let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
arr[0].count++;

this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId) // 可看Pos组件代码
```

### Element组件的$message方法
> 可直接在方法中使用
```javascript
this.$meaasge.success('.....');
this.$message.error('....');
```
也可以这样
```javascript
this.$message({
message: '结账成功，感谢你的购买！',
type: 'success'
})
```

### css peek问题
> 在点击TDTradeBuy页面的rote-select样式的时候，只跳转到merge.min.css文件，解决： 点击右上角“为此文件禁用换行”，就会出现文件给你选择


### vue项目打包上线
> config/index.js的assetsPublicPath公用路径的/改为./相对路径
运行 npm run build 会生成一个build文件。可直接将此文将放到服务器使用