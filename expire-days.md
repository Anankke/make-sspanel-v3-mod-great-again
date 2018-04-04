# 在用户中心首页显示账户剩余天数和套餐剩余天数

在 `resources/views/material/user/index.tpl` 中，`<script src="/theme/material/js/shake.js/shake.js"></script>` 后面插入以下代码：

```html
<script>
/*
 * Author: neoFelhz & CloudHammer
 * https://github.com/CloudHammer/CloudHammer/make-sspanel-v3-mod-great-again
 * License: MIT license & SATA license
 */
function CountDown() {
    var levelExpire = Date.parse("{$user->class_expire}");
    var accountExpire = Date.parse("{$user->expire_in}");
    var nowDate = new Date();
    var a = nowDate.getTime();
    var b = levelExpire - a;
    var c = accountExpire - a;
    var levelExpireDays = Math.floor(b/(24*3600*1000));
    var accountExpireDays = Math.floor(c/(24*3600*1000));
    if (levelExpireDays < 0 || levelExpireDays > 315360000000) {
        document.getElementById('days-level-expire').innerHTML = "无限期";
        for (var i=0;i<document.getElementsByClassName('label-level-expire').length;i+=1){
            document.getElementsByClassName('label-level-expire')[i].style.display = 'none';
        }
    } else {
        document.getElementById('days-level-expire').innerHTML = levelExpireDays;
    }
    if (accountExpireDays < 0 || accountExpireDays > 315360000000) {
        document.getElementById('days-account-expire').innerHTML = "无限期";
        for (var i=0;i<document.getElementsByClassName('label-account-expire').length;i+=1){
            document.getElementsByClassName('label-account-expire')[i].style.display = 'none';
        }
    } else {
        document.getElementById('days-account-expire').innerHTML = accountExpireDays;
    }
}
</script>
```

在你需要显示等级有效期和账户有效期的地方分别创建下述 element：

```html
<!--
    以下是一个代码样例
-->
<!-- 等级有效期 -->
<span class="label-level-expire">剩余</span>
<span id="days-level-expire"></span>
<span class="label-level-expire">天</span>

<!-- 账户有效期 -->
<span class="label-account-expire">剩余</span>
<span id="days-account-expire"></span>
<span class="label-account-expire">天</span>
```

默认显示效果为 `剩余 xx 天`，当剩余有效期大于 10 年时会显示 `无限期`，`剩余` `天` 会被隐藏。

> 你可以自定义显示效果的样式，但是需要保留元素的 `id="days-level-expire"` 和 `class="label-account-expire"` 用于 querySelector 选取。

在接下来的 `<script>` 标签中找到：

```javascript
..
window.onload = function() {
    var myShakeEvent = new Shake({
        threshold: 15
    });
    myShakeEvent.start();

    window.addEventListener('shake', shakeEventDidOccur, false);
...
```

在 `myShakeEvent.start();` 之后加上 `CountDown()`，如下所示：

```diff
 ...
 window.onload = function() {
     var myShakeEvent = new Shake({
         threshold: 15
     });
     myShakeEvent.start();
+    CountDown();

     window.addEventListener('shake', shakeEventDidOccur, false);
 ...
```

> `CountDown()` Function 需要在 `windows.onload` 事件触发之后调用，除了如上述方式现有的 `window.onload` 中进行 patch，你也可以用自己的方式实现。
