## ng-if 和 ng-show
ng-show:
> 指令在表达式为 true 时显示指定的 HTML 元素，否则隐藏指定的 HTML 元素。

即将该元素的css属性display设置为none;

ng-if:
> ng-if 指令用于在表达式为 false 时移除 HTML 元素。
  如果 if 语句执行的结果为 true，会添加移除元素，并显示。
  ng-if 指令不同于 ng-hide， ng-hide 隐藏元素，而 ng-if 是从 DOM 中移除元素。

即直接移除dom树上的节点。

### 开发中遇到的问题
需求：点击红色框时展示，再次点击收回，点击灰色区域也是收回；
![资金明细](./images/fundDetail.jpg)
```javascript
<div class="fund-dialog" ng-show="showTypeDialog" ng-click="showTypeDialog = false">
        <ul>
            <li class="{{currType}}" ng-repeat="(key, value) in fundTypeList" ng-click="confirmFundType(key);">
                <span class="{{key}}">{{value.text}}</span>
            </li>
        </ul>
</div>
```
代码中若使用ng-if的话,ng-click无作用,但是若ng-click调用该方法，方法包含
```javascript
showTypeDialog = false
```
则有作用.
若用ng-show就没有这个问题
于是这里爆出一个问题：当ng-click执行的方法为修改状态并删除本身时，为什么无作用，是因为ng-click内直接写了方法的问题还是ng-if得问题。
网上也没找到相应的问题.
后续解决的话就补上

### 反转状态时，若只是修改true / false,则不需使用三目
```javascript
$scope.showTypeDialog = !$scope.showTypeDialog;
```

