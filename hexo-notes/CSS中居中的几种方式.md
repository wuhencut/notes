## 水平居中
#### 1、margin: 0 auto;
关于这个，大家也不陌生做网页让其居中用的比较多,
这个是用于子元素上的，前提是**不受float影响**
```javascript
<style>
    *{
        padding: 0;
        margin: 0;
    }
        .box{
            width: 300px;
            height: 300px;
            border: 3px solid red;
        }
        img{
            display: block;
            width: 100px;
            height: 100px;
            margin: 0 auto;
        }
    </style>

<!--html-->
<body>
    <div class="box">
        ![](img1.jpg)
    </div>
</body>
```
#### 2、text-align：center；
img的display：block；类似一样在不受float影响下进行
实在父元素上添加效果让它进行水平居中
```javascript
<style>
    *{
        padding: 0;
        margin: 0;
    }
        .box{
            width: 300px;
            height: 300px;
            border: 3px solid red;
            text-align: center;
        }
        img{
            display: block;
            width: 100px;
            height: 100px;
        }
    </style>

<!--html-->
<body>
    <div class="box">
        ![](img1.jpg)
    </div>
</body>
```

#### 3、水平垂直居中（一）定位和需要定位的元素的margin减去宽高的一半
这种方法的局限性在于需要知道需要垂直居中的宽高才能实现，经常使用这种方法
```javascript
 <style>
        *{
            padding: 0;
            margin: 0;
        }
        .box{
            width: 300px;
            height: 300px;
            background:#e9dfc7;
            border:1px solid red;
            position: relative;
        }
        img{
            width: 100px;
            height: 150px;
            position: absolute;
            top: 50%;
            left: 50%;
            margin-top: -75px;
            margin-left: -50px;
        }
    </style>
<!--html -->
<body>
    <div class="box" >
        ![](2.jpg)
    </div>
</body>
```
#### 4.水平垂直居中（二）定位和margin:auto;
这个方法也很实用，不用受到宽高的限制,也很好用
```javascript
<style>
        *{
            padding: 0;
            margin: 0;
        }
        .box{
            width: 300px;
            height: 300px;
            background:#e9dfc7;
            border:1px solid red;
            position: relative;

        }
        img{
            width: 100px;
            height: 100px;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            margin: auto;
        }
    </style>
<!--html -->
<body>
    <div class="box" >
        ![](3.jpg)
    </div>
</body>
```
水平垂直居中2

#### 5.水平垂直居中（三）绝对定位和transfrom
这个方法比较高级了，用到了形变，据我所知很多大神喜欢使用这个方法进行定位，逼格很高的，学会后面试一定要用！
```javascript
<style>
    *{
        padding: 0;
        margin: 0;
    }
    .box{
        width: 300px;
        height: 300px;
        background:#e9dfc7;
        border:1px solid red;
        position: relative;

    }
    img{
        width: 100px;
        height: 100px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
</style>
<!--html -->
<body>
    <div class="box" >
        ![](4.jpg)
    </div>
</body>
```
水平垂直居中3

#### 6.水平垂直居中（四）diplay：table-cell
其实这个就是把其变成表格样式，再利用表格的样式来进行居中，很方便
```javascript
<style>
    .box{
            width: 300px;
            height: 300px;
            background:#e9dfc7;
            border:1px solid red;
            display: table-cell;
            vertical-align: middle;
            text-align: center;
        }
        img{
            width: 100px;
            height: 150px;
            /*margin: 0 auto;*/  这个也行
        }
</style>
<!--html -->
<body>
    <div class="box" >
        ![](5.jpg)
    </div>
</body>
```
水平垂直居中4

#### 7.水平垂直居中（五）flexBox居中
这个用了C3新特性flex,非常方便快捷，在移动端使用完美，pc端有兼容性问题，以后会成为主流的
```javascript
<style>
    .box{
            width: 300px;
            height: 300px;
            background:#e9dfc7;
            border:1px solid red;
            display: flex;
            justify-content: center;
            align-items:center;
        }
        img{
            width: 150px;
            height: 100px;
        }
</style>
<!--html -->
<body>
    <div class="box" >
        ![](6.jpg)
    </div>
</body>
```
垂直居中5


