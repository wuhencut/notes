# Http缓存相关

<a name="2135a9a1"></a>
# 
缓存分**强缓存**和**协商缓存，**强缓存(浏览器缓存)怎么控制**：**<br />**强缓存**<br />

- Cache-Control：参数
   - max-age 缓存时间(秒)
   - no-cache 不走强缓存，与服务器协商缓存是否新鲜
   - no-store 不走强缓存也不走协商缓存，我就要新资源，就是这么giao
   - private：专用于个人的缓存，中间代理、CDN 等不能缓存此响应
   - public：响应可以被中间代理、CDN 等缓存
   - must-revalidate：在缓存过期前可以使用，过期后必须向服务器验证 (这几个用的太少了，我也不记，有点印象即可)
- Expires 最没用，一般不用：系统时间跟Expires比较，如果早点，就去那浏览器缓存，超过了就去拿服务器的。但是，我要是改了服务器时间就GG，所以会导致误差，一般不用
- Pragma 参数只有 no-cache 但是优先级比cache-control高，有了Pragma，Cache_control一边去


<br />**协商缓存**<br />

- 主要俩参数Etag和LastModified
   - Etag是资源文件的哈希值，用来跟服务器资源文件匹配，不匹配就走新资源，匹配就滚回去拿浏览器缓存(304)的。
   - LastModified 字面意思，最近一次修改(的时间) Date格式的 跟服务器上次修改时间进行比较，相同也滚回去拿浏览器的缓存


<br />Point：Etag比LastModify重要，因为ETag代表了文件的哈希值。<br />
<br />参考：[https://juejin.im/post/5eb7f811f265da7bbc7cc5bd](https://juejin.im/post/5eb7f811f265da7bbc7cc5bd)<br />
<br />
<br />人脑不同，理解深度不同，欢迎补充。
