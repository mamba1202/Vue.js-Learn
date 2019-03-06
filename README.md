#### 数据绑定及指令(vue.js-02)
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

#### 过滤器，指令，事件，语法糖。(vue.js-02)
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
        //v-text指令-解析文本 和{{}}一样
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
            //value 写不写都存在
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
#### 计算属性将字符串反转(vue.js-03)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="demo">
        {{text}}<br>
        {{text.split(',').reverse().join(',')}}
        逻辑过长就会变得臃肿，难以维护，所以遇到复杂的逻辑时，应当使用计算属性
        <hr>
        下边是使用计算属性得到的<br>
        {{reverseText}}
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        //将'123,456,789'变成'789,456,123'
        var app = new Vue({
            el: "#demo",
            data: {
                text: "123,456,789"
            },
            //与data同级-定义计算属性
            computed: {
                reverseText: function () {
                    return this.text.split(',').reverse().join(',')
                }
            }
        })
    </script>
</body>

</html>
```
#### 计算属性购物车总价(vue.js-03)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="demo">
        两个购物车总计{{prices}}
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        //计算购物车中的所有物品价格
        var app = new Vue({
            el: "#demo",
            data: {
                //第一个购物车
                package1: [
                    {
                        name: 'iphone',
                        price: 6999,
                        count: 2
                    },
                    {
                        name: 'iPad',
                        price: 3600,
                        count: 1
                    }],
                //第二个购物车
                package2: [
                    {
                        name: 'iphone8',
                        price: 6566,
                        count: 3
                    },
                    {
                        name: 'iPad',
                        price: 3600,
                        count: 6
                    }]
            },
            //与data同级-定义计算属性
            computed: {
                prices: function () {
                    var prices = 0;
                    for (var i = 0; i < this.package1.length; i++) {
                        prices += this.package1[i].price * this.package1[i].count;
                    }
                    for (var j = 0; j < this.package2.length; j++) {
                        prices += this.package2[j].price * this.package2[j].count;
                    }
                    return prices;
                }
            }
        })
    </script>
</body>

</html>
```
### get与set(vue.js-03)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="demo">
        {{fullName}}
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var app = new Vue({
            el: "#demo",
            data: {
                firstName: 'zhang',
                lastName: 'zhe'
            },
            computed: {
                //计算属性默认用法是getter函数
                fullName: function () {
                    return this.firstName + " " + this.lastName
                },
                get: function () {
                    return this.firstName + " " + this.lastName
                },
                set: function (newValue) {
                    console.log('我是set方法，我被调用')
                    var names = newValue.split(','); //分隔为数组
                    this.firstName = names[0];
                    this.lastName = names[1];
                }
            }
        })
    </script>
</body>

</html>
```
#### 计算属性与methods方法(vue.js-03)

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="demo">
        {{text}}<br>
        {{text.split(',').reverse().join(',')}}
        逻辑过长就会变得臃肿，难以维护，所以遇到复杂的逻辑时，应当使用计算属性
        <hr>
        下边是使用计算属性得到的<br>
        {{reverseText}}
        <hr>
        计算属性的缓存{{now}}
        <hr>
        通过methods拿到的时间戳(方法要加括号)
        {{thisTime()}}
       
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        //将'123,456,789'变成'789,456,123'
        var app = new Vue({
            el: "#demo",
            data: {
                text: "123,456,789"
            },
            //与data同级-定义计算属性
            computed: {
                reverseText: function () {
                    return this.text.split(',').reverse().join(',')
                },
                now: function () {
                    return Date.now();
                }
            },
            methods: {
                thisTime: function () {
                    return Date.now();
                }
            }

        })
    </script>
</body>

</html>
```
#### v-bind复习(vue.js-04)
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <a v-bind:href="url">我是百度</a>
        <img :src="imgUrl" alt="">
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
var app = new Vue({
    el: '#app',
    data:{
        url: 'http://baidu.com',
        imgUrl: "图片链接地址"
    }
})
</script>
</body>
</html>
```
#### class和style的绑定(vue.js-04)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .divStyle {
            background-color: blue;
            height: 100px;
            width: 100px;
        }

        .borderStyle {
            border: 10px solid red;
        }

        .btnBackground {
            background-color: darkred;
        }

        .active {
            background-color: black;
            height: 100px;
            width: 100px;
        }

        .error {
            border: 10px solid blue;
        }
    </style>
</head>

<body>
    <div id="app">
        绑定class对象语法，对象的键是类名，值是布尔值
        <hr>
        <div :class="{divStyle : isActive, borderStyle : isBorder}"></div>
        <hr>
        <input type="button" value="点击我切换背景，哈哈" :class={btnBackground:isback} v-on:click="changColor">
        /*<div :class="className"></div>*/
        //计算属性
        绑定class数组语法：数组中的成员直接对应类名<br>
        <div :class="[activeClass,errorClass]">我是数组绑定class</div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true,
                isBorder: true,
                isback: true,
                activeClass: "active",
                errorClass: "error"
            },
            methods: {
                changColor: function () {
                    this.isback = !this.isback;
                }
            },
            computed: function () {
                return {
                    active: this.isActive && !this.isBorder
                    //对象语法 键是类名，值是布尔
                }
            }
        })
    </script>
</body>

</html>
```
#### 数组和对象混用(vue.js-04)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .divStyle {
            background-color: blue;
            height: 100px;
            width: 100px;
        }

        .borderStyle {
            border: 10px solid red;
        }

        .btnBackground {
            background-color: darkred;
        }

        .active {
            background-color: black;
            height: 100px;
            width: 100px;
        }

        .error {
            border: 10px solid blue;
        }
    </style>
</head>

<body>
    <div id="app">
        数组和对象混用，第一个成员是对象，第二个成员是数组成员
        <div :class="[{'active' : isActive},errorClass]"></div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true,
                errorClass: "error"
            },
        })
    </script>
</body>

</html>
```
#### style对象语法与数组语法(vue.js-04)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        style对象语法
        绑定内联样式：键代表style的属性，值代表属性对应的值
        <hr>
        vue中只要是大写字母都会转换成-加小写
        <div v-bind:style="{'color':color,'fontSize':fontSize+'px'}">哈哈</div>
        <hr>
        style数组语法
        <div v-bind:style="[styleA,styleB]">
            哈哈哈哈哈哈哈
        </div>

    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                color: 'blue',
                fontSize: 18,
                styleA: {
                    color: red,
                    fontSize: 18px,
                }
            },
        })
    </script>
</body>

</html>
```
#### 基本指令 v-clock和 v-once(vue.js-05)
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        v-clock:解决初始化慢导致页面闪动<br>
        <p v-once>{{msg}}</p>
        <hr>
        v-once:只渲染一次，后续都不会再渲染<br>
        <span v-once>{{oncedata}}</span>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data:{
                msg: "湖人总冠军",
                oncedata: "紫金王朝"
            }
        })
    </script>
</body>

</html>
```