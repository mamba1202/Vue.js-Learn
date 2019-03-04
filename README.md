####数据绑定及指令
```
!DOCTYPE html>
<html lang="en">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
</head>

<body>
<div id="app">
{{msg}}<br>
{{ 6+6-18}}<br>
{{ 6>3 ? msg : a}}

</div>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
//环境搭建
<script>
var app = new Vue({
el: "#app",//Element用于指定页面中已经存在的DOM元素，挂载到DOM上(el可以是元素也可是class/id)
data: {
msg: "conde社区",
a: 2
},
created: function(){
//alert('我是vue实例，创建完成，还未挂载到dom')
},
mounted: function(){
//alert('我是vue实例，已经挂载到dom')
},
});
//访问vue实例的属性
console.log(app.$el)
console.log(app.$data)
//访问data中的属性
console.log(app.msg)
console.log(app.a)
</script>
</body>

</html>
```

#### 过滤器，指令，事件，语法糖。
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .mamba {
            background-color: red;
            height: 20px;
        }
    </style>
</head>

<body>
    <div id="dataApp">
        {{date}}<br>
        {{date | formatDate}}
        <hr>
        {{apple}}<br>
        <span v-text="apple"></span>
        <hr>
        <span v-html="banana"></span>
        //v-html指令-解析HTML
        <hr>
        <div v-bind:class="className"></div>
        //v-bind指令- 绑定活的属性
        <hr>
        <button v-on:click="count">{{countnum}}</button>
        //v-on指令-绑定事件监听器
        //为按钮添加监听事件
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var plusDate = function (value) {
            return value < 10 ? '0' + value : value
        }
        var App = new Vue({
            el: "#dataApp",
            data: {
                date: new Date(),
                apple: "苹果",
                banana: '<span style="color: yellow;">香蕉</span>',
                className: "mamba",
                countnum: 0
            },

            filters: {
                //这里的value就是需要过滤的数据
                formatDate: function (value) {
                    //将字符串转化为data类型
                    var date = new Date(value);
                    var year = date.getFullYear(); //年
                    var month = plusDate(date.getMonth() + 1); //月
                    var day = plusDate(date.getDate()); //日
                    var hours = plusDate(date.getHours());
                    var min = plusDate(date.getMinutes());
                    var sec = plusDate(date.getSeconds());
                    //将整理好的数据返回
                    return year + '--' + month + '--' + day + '  ' + hours + '--' + min + '--' + sec
                }
            },
            methods: {
                count: function () {
                    this.countnum = this.countnum + 1
                }
            },
            mounted: function () {
                var _this = this //this代表vue实例
                this.timer = setInterval(function () {
                    _this.date = new Date();
                }, 1000)
            },
            beforeDestroy: function () {
                //如果定时器存在，就清除定时器
                if (this.timer) {
                    clearInterval(this.timer)
                }
            }
        })
    </script>
</body>

</html>
```