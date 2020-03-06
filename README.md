# 一个基于vue实现的时间倒计时组件

## Install

```
npm i vue-count-down-time
```

```
import vueCountdownTime from 'vue-count-down-time';
Vue.use(vueCountdownTime);

```

## 在组件内可以传各种参数，支持的参数如下
```
startTime: {
    type: String | Number,
    default: ''
},
startTimeFormat: {
    type: String,
    default: '%Y-%M-%d %h:%m:%s'
},
endTime: {
    type: String | Number,
    default: ''
},
endTimeFormat: {
    type: String,
    default: '%Y-%M-%d %h:%m:%s'
},
startCallBk: {
    type: Function,
    default: null
},
endCallBk: {
    type: Function,
    default: null
},
showTemplate: {
    type: String,
    default: '${day}天 ${hour}小时 ${min}分钟 ${sec}秒'
},
timeInterval: {
    type: String | Number,
    default: 1000
}
```
### Example

```
<!-- 支持自定义模板 showTemplate参数 -->
<vue-count-down-time startTime="2020-03-07 20:00:00" endTime="1583586000000"  showTemplate="<span class='bb'>${day}</span><span class='bb'>${hour}</span><span class='bb'>${min}</span><span class='bb'>${sec}</span>" :endCallBk="endCallBk"></vue-count-down-time>
```

```
<!--间隔2s 执行一次 有开始回调 和结束回调-->
<vue-count-down-time startTime="2020-03-07 20:59:50" endTime="2020-03-07 21:00:00" :startCallBk="startCallBk" :endCallBk="endCallBk" timeInterval="2000" ></vue-count-down-time>
    
```