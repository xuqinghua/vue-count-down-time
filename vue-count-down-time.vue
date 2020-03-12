<template>
    <div class="vue-count-time" v-html="leftTimeHtml"></div>
</template>

<script>
export default {
    props: {
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
    },
    data() {
        return {
            msec: 0,
            day: '',
            hour: '',
            min: '',
            sec: '',
            leftTimeHtml: '',
            setTimeoutFunc:null
        };
    },
    created() {
        const start = this.getTimeStamp(this.startTime, this.startTimeFormat);
        const end = this.getTimeStamp(this.endTime, this.endTimeFormat);
        this.msec = end - start;
        if (this.startCallBk){
            this.startCallBk();
        }
        this.countDownTimer();
        //组件销毁的时候，清除掉定时器
        this.$once('hook:beforeDestroy', () => {            
            this.setTimeoutFunc && clearTimeout(this.setTimeoutFunc);                                    
        });
    },
    methods: {
        getTimeStamp(time, timeFormat){
            if (typeof time === 'number') {
                if (time.toString().length === 10) {
                    return time * 1000;
                } else {
                    return time;
                }
            } else {
                try {
                    return this.str2Date(time, timeFormat).getTime();
                } catch (e){
                    return parseInt(time, 10);
                }
            }
        },
        countDownTimer() {
            let day = parseInt(this.msec / 1000 / 60 / 60 / 24, 10);
            let hour = parseInt(this.msec / 1000 / 60 / 60 % 24, 10);
            let min = parseInt(this.msec / 1000 / 60 % 60, 10);
            let sec = parseInt(this.msec / 1000 % 60, 10);
            this.day = day > 9 ? day : '0' + day;
            this.hour = hour > 9 ? hour : '0' + hour;
            this.min = min > 9 ? min : '0' + min;
            this.sec = sec > 9 ? sec : '0' + sec;

            let showHtml = this.showTemplate.replace('${day}', this.day).replace('${hour}', this.hour).replace('${min}', this.min).replace('${sec}', this.sec);
            this.leftTimeHtml = showHtml;
            if (this.msec <= 0){
                this.endCallBk && this.endCallBk();
                return;
            }
            this.setTimeoutFunc =  setTimeout( () => {
                this.msec = this.msec - this.timeInterval;
                this.countDownTimer();
            }, this.timeInterval);
        },
        str2Date(date, p) { // 表示日期的值， p表示日期的格式
            if (!date) {return null;}
            p = p || '%Y-%M-%d %h:%m:%s';
            p = p.replace(/\-/g, '\\-');
            p = p.replace(/\|/g, '\\|');
            p = p.replace(/\./g, '\\.');
            p = p.replace(/\+/g, '\\+');
            p = p.replace('%Y', '(\\d{4})');
            p = p.replace('%M', '(\\d{1,2})');
            p = p.replace('%d', '(\\d{1,2})');
            p = p.replace('%h', '(\\d{1,2})');
            p = p.replace('%m', '(\\d{1,2})');
            p = p.replace('%s', '(\\d{1,2})');

            var regExp = new RegExp('^' + p + '$'),
                group = regExp.exec(date),
                Y = (group[1] || 0) - 0,
                M = (group[2] || 1) - 1,
                d = (group[3] || 0) - 0,
                h = (group[4] || 0) - 0,
                m = (group[5] || 0) - 0,
                s = (group[6] || 0) - 0;

            return new Date(Y, M, d, h, m, s);
        }
    }
};
</script>
