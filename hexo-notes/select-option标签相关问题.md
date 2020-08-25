### select/option无法监听点击事件
#### ng-model
> ng-model是指下拉表单的默认显示。

#### 遇到的问题：
> 项目需要做一个下拉列表，列表中每个选项都含有两个值，这两个值都是后台获取，分别有不同的作用。刚开始用了ng-repeat来做，能正常展示，但是发现click事件居然没用。
于是查到，ng-options这个东西也是很坑。下面先介绍下这个标签

#### ng-options
> ng-options 指令用于使用 <options> 填充 <select> 元素的选项，当select中一个选项被选择，该选项将会被绑定到ng-model。也就是这里也是双向绑定的

他有多种用法：
> 对于数组：
   label for value in array 直接显示内容
   select as label for value in array   //value是个对象， select当作label显示，也就是做，label还是label，select是label的索引(nmmp select = label),项目中用到的便是这个
   label group by group for value in array //label group是group的子集  下面的基本不常用，用到了自己试吧
   label disable when disable for value in array
   label group by group for value in array track by trackexpr
   label disable when disable for value in array track by trackexpr
   label for value in array | orderBy:orderexpr track by trackexpr(for including a filter with track by)
  对于对象：
   label for (key , value) in object
   select as label for (key ,value) in object
   label group by group for (key,value) in object
   label disable when disable for (key, value) in object
   select as label group by group for(key, value) in object
   select as label disable when disable for (key, value) in object

所以，根据他的用法，我又尝试了一遍，发现！！mmp，select as label for value in array 用这个其实也是只能显示一个

这是代码：
```javascript
//处理止盈线列表                 止盈线列表  占用点买金
    function processHighMoneyList(list, totalMoney){
        var highMoneyList = [];//这个用来处理遍历的列表
        var rote, money;
        list.forEach(function(item){
            rote = item;
            money = X.toInt((item - 0) / 100 * totalMoney);
            highMoneyList.push({
                rote: rote, // 这个作为index
                money: rote + '%,' + money + '元' //这个是显示的内容
            });
        });
        $scope.highMoneyList = highMoneyList;
        $scope.defaultRatio = quitGainRatioList.split('|')[1];//默认的rote
        schemeStrategyQuitGain = X.toInt(($scope.defaultRatio - 0) / 100 * $scope.totalStockMoney);//这个是用来处理默认显示的金额的
    }
    var schemeStrategyQuitGain = 0;
```
点击事件：
```javascript
//选择止盈
    $scope.chooseHigh = function (rote){
        schemeStrategyQuitGain = X.toInt((rote - 0) / 100 * $scope.totalStockMoney);
    };
```
页面代码:
```javascript
                  //绑定的默认值作为index指向他的值                               index       value                                          事件              双向绑定的变量
<select ng-model="defaultRatio" class="txt-orange rote-select" ng-options="item.rote as item.money for item in highMoneyList" ng-change="chooseHigh(defaultRatio)"></select>
```

到此，才算解决了这个问题
