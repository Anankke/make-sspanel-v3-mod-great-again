# 在账户信息页面显示历史累计返利金额

在 `resources/views/material/user/profile.tpl` 中找到并修改：

```diff
<div class="card-table">
    <div class="table-responsive">
        {$paybacks->render()}
            <table class="table">
                <thead>
                    <tr>
                        <th>###</th>
                        <th>返利用户</th>
                        <th>返利金额</th>
                    </tr>
                </thead>
                <tbody>
                    {foreach $paybacks as $payback}
                    <tr>
                        <td><b>{$payback->id}</b></td>
                        {if $payback->user()!=null}
                            <td>{$payback->user()->user_name}</td>
                        {else}
                            <td>已注销</td>
                        {/if}
-                        <td>{$payback->ref_get} 元</td>
+                        <td><span class="payback-num">{$payback->ref_get}</span> <span>元</span></td>
                    </tr>
                    {/foreach}
                </tbody>
            </table>
        {$paybacks->render()}
        </div>
    </div>
</div>
```

然后在 `{include file='user/footer.tpl'}` 之前加上这段：

```html
<script>
/*
 * Author: neoFelhz & CloudHammer
 * https://github.com/CloudHammer/CloudHammer/make-sspanel-v3-mod-great-again
 * License: MIT license & SATA license
 */
function countPayBack() {
    var b = 0;
    for (var i=0;i<document.getElementsByClassName('payback-num').length;i+=1){
        var a = parseFloat(document.getElementsByClassName('payback-num')[i].innerHTML);
        if (a > 0) {
            var b = a + b;
        } else {
            var b = b;
        }
    }
    document.getElementById('payback-num-all').innerHTML = b.toFixed(2);
}
window.onload = function() {
    countPayBack();
};
</script>
```

在你想要显示累计返利金额的地方添加 id 为 `payback-num-all` 的 element。

```html
<!--
    以下是一个代码样例
-->
<!-- 输出累计返利金额 -->
累计返利金额： <span class="payback-num-all"></span> 元
```
