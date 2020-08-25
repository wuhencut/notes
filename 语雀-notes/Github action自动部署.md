# Github action实现自动部署

<a name="pKDPo"></a>
### 目的：懒得手动拉文件去oss保存，不会jenkins。
<a name="dWCMz"></a>
### 限制：每个月有构建时间限制，大项目最后还是要用jenkins或者其他构建工具。
<a name="n3DWY"></a>
### 缺点：构建时间太慢了。一个普通的文件中代码的更改，actions中要等待2分钟左右，不发牢骚了。说内容，怎么也折腾了我几个小时了。不会yml，不会docker的痛，不会node，不会linux的痛。

- **首先就是在github - actions中构建一个workflow**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1598343257710-3f380196-a7ab-4628-9e28-1900a4b8b9e4.png)

- ![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1598343291169-ed5638d4-badf-40f1-a4a1-65737a7a1e86.png)**选择一个模板**
- **改这个yml文件**
- **需要拿到阿里云的access key 和id id你可以看到，但是key默认是隐藏的，所以你需要新建一个，会弹窗，只给你看一次。**

**![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1598343646487-caa5b083-42ba-4efc-bec5-1d0182f4558b.png) ![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1598343678862-9797fd3a-1292-45bf-8223-f460f33e51e7.png)**<br />****把id和key都复制下来，到github 对应目录下的setting - secrets中创建 id和key就好了 看图吧<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/126454/1598343800944-b27737fe-748a-4fa9-aa05-3fe6c78d19b9.png)****
```yaml
# 在action的构建进度中展示的名字
name: upload oss
# 构建到master分支
on:
  push:
    branches:
      - master

jobs:
  build:
		# 这是固定死的，暂时不懂
    runs-on: ubuntu-latest

    # strategy:
    #   matrix:
    #     node-version: [8.x, 10.x, 12.x]

    steps:
      #拉取代码
      - name: Checkout
      	# 这个action 就是检出最近代码
        uses: actions/checkout@master
      - name: setup aliyun oss
      	# 这个action就是把github的代码，检出到aliyun，所以要用到阿里云的access-key和access-id
        uses: manyuanrong/setup-ossutil@master
        with:
          endpoint: oss-cn-hangzhou.aliyuncs.com
          access-key-id: ${{ secrets.ALIYUN_ACCESS_KEY_ID }}
          access-key-secret: ${{ secrets.ALIYUN_ACCESS_KEY_SECRET }}

      - name: cp files to aliyun
      	# 进入目录，把左右文件 推送到 timing-web根目录下
        # 如果是把xx 目录下的文件推送到 oss 的yy bucket 下的zz文件夹下就是:
        # run: ossutil cp ./xx oss://yy/zz -rf
        run: ossutil cp ./ oss://timing-web -rf

```
