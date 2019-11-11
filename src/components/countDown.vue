<template>
  <div class="countDown">
    <div v-if="!isCountDownOver">
      <span class="title">抢购倒计时</span>
      <span class="time">{{countDownMinutes}}</span>min
      <span class="time">{{countDownSeconds}}</span>s
    </div>
    <div v-else class="timeOver">
      抢购已结束
    </div>
  </div>
</template>

<script>
export default {
  name: 'countDown',
  data () {
    return {
      isCountDownOver: false,
      countDownMinutes: '00',
      countDownSeconds: '00'
    }
  },
  mounted () {
    this.timer()
  },
  methods: {
    /**
     * 时间格式化
     */
    timeFormat(param) {
      return param < 10 ? '0' + param : param
    },
    /**
     * 倒计时功能
     */
    timer () {
      // new Date() 可自定义为抢购时间
      let failTime = new Date().getTime() + 1 * 60 * 1000 // 抢购失效时间
      let nowTime = new Date().getTime() // 当前时间
      let leftTime = failTime - nowTime
      let interval = setInterval(() => {
        let timeObj = {}
        leftTime = leftTime - 1000
        if (leftTime > 0) {
          let minutes = parseInt(leftTime / 1000 / 60 % 60)
          let seconds = parseInt(leftTime / 1000 % 60)
          timeObj = {
            minutes: this.timeFormat(minutes),
            seconds: this.timeFormat(seconds)
          }
        }else {
          this.isCountDownOver = true
          clearInterval(interval) // 清除定时器
        }
        this.countDownMinutes = timeObj.minutes
        this.countDownSeconds = timeObj.seconds
      }, 1000);
    },
  }
}
</script>

<style>
.countDown {
  width: 175px;
  font-size: 14px;
  border: 1px solid #ccc;
  padding: 5px 10px;
}
.countDown .title {
  font-weight: 700;
  color: #c00;
  margin-right: 5px;
}
.countDown .time {
  background-color: #000;
  color: #fff;
  margin: 0 2px;
  padding: 0 5px;
  border-radius: 3px;
}
.timeOver {
  font-weight: 700;
  color: #c00;
}
</style>