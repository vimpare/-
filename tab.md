隐藏tab栏：
```
swan.hideTabBar({
    animation: true,
    success: function () {
        console.log('hideTabBar success');
    },
    fail: function (err) {
        console.log('hideTabBar fail', err);
    }
});
```
