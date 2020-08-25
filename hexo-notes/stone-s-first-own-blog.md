## 我第一篇blog遇到的问题
### markdown语法基本规范:
[规范](http://www.coderli.com/write-readme-for-your-project/)
### 编辑器卡死
上周五到今天早上一直遇到的一个导致编辑器卡死的问题：scanning files to index;WTF?!导致我的webStorm完全不能动弹！原因竟然是下载的node_modules太大了……扫描半天导致编辑器卡死；
后来网上终于找到解决办法：node_modules右键--> Mark directory as --> Exclude x  完美解决！
### 打开webStorm卡死。
删除.idea;
### hexo建站准备：
[link](https://hexo.io/zh-cn/docs/index.html)
安装node.js,git,cnpm
### cnpm:
https://npm.taobao.org/
优点：下载速度比npm快；
安装指令：
```javascript
 npm install -g cnpm --registry=https://registry.npm.taobao.org
```
### 引用别人主题时要注意：
安装一些必须的包，例如jade，sass等，避免报错。
### 若点击页面功能报错：
可能原因：引用了谷歌的jquery，解决方法：修改对应主题下的layout-->_partial-->after-footer.ejs的script引用百度静态资源公共库的jquery库（http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js ）；
内容注意：上句话中的script不能写成标签形式，因为markdown是支持html标签的，所以在写博过程中经常会遇到带网页标签的文章内容和Markdown排版关键字冲突；
## 发布时注意：
### 1、修改配置文件中的deploy下的配置
```javascript
type: git
repo: https://github.com/wuhencut/wuhencut.github.io.git  (路由是对象的github地址或者存放项目的域名，在github上对应项目clone下网址即可)，访问自己定义的域名就可以(wuhencut.github.io.git)
branch: master
message:
```
### 2、必须进行编译生成public文件；hexo g
### 3、编译完成后用hexo d发布；
### 在.gitignore文件中加.idea/   public/    .delopy_git/
### 引用块
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

### 何时须重启hexo
1、修改主题的config文件后必须重启，不然会报错。（这里出现了一个让我蛋疼的事情，我真的是太粗心了。我要添加links这个widget的时候，因为之前把links注释了，我把widgets下的links解释了，links下的链接也解释了，重点来了！links这个头我特么没有解释！毙了狗，一直报错，找半天，哎，还是太粗心了，希望大家还是要仔细啊！）

2、发布版本前也必须重启hexo，同样会报错。

**加粗字体**

用四个星号即可实现，不用大小和加粗与四号标题一样

[百度](http://www.baidu.com)

> 文字引用：
```javascript
> content
```
### 代码块高亮：ruby
```javascript
  def add(a, b)
    return a + b
  end
```

### 代码块正常显示：bash
```javascript
function(){
    alert(hello world!);
}
```

### 插入图片
语法：
```javascript
![name](imageUrl)
```
注意：相对路径引用图片时，路由最前面必须要有/斜杠
![福克纳](./images/fokena.jpg)














