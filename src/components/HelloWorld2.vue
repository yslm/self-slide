<template>
  <div>
    <section class="wh_content" @touchmove="fn">
      <div :class="className" class="wh_swiper" @touchstart="s" @touchmove="m" @touchend="e">
        <div class="wh_slide" v-for="val in 5">
          {{val}}
        </div>
      </div>

    </section>


    <TestSwiper></TestSwiper>
  </div>

</template>

<script>

  import TestSwiper from './test-swiper'

  export default {
    data() {
      return {
        slidesLength: 1, //滑块的个数，默认是一个
        _width: 0,   // ul的宽度，和外盒子宽度一样
        auto: false,
        slideing: true,
        timer1: '',
        className: '',
        dom: {},  //代表ul的样式
        t: {
          sx: 0, //初始移动的距离
          s: 0, //touchu的初始位置  touches[0].clientX
          m: 0,  //滑动的距离 也就是moveX
          e: 0
        },
        index: 1
      }
    },
    props: {
      //滑动所需要的时间
      autoPlay: {
        default: true
      },
      //一次滑动需要走多久
      duration: {
        default: 500
      },
      //两次滑动间隔的时间
      interval: {
        default: 2500
      },
      showIndicator: {
        default: true
      }
    },
    components:{
      TestSwiper
    },
    methods: {
      s(x) {
        if (this.slideing) {
          this.clearTimeOut()
          this.t.sx = this.getTransform()  //初始移动的距离
          console.log(this.getTransform(),'初始值');
          this.t.s = x.touches[x.touches.length - 1].clientX  //记录初始接触的位置
        }
      },
      m(x) {
        if (this.slideing && this.t.s != -1) {
          this.clearTimeOut()
          this.t.m = x.touches[x.touches.length - 1].clientX - this.t.s
          this.setTransform(this.t.m + this.t.sx)  //？？
        }
      },
      e(x) {
        if (this.slideing && this.t.s != -1) {
          this.clearTimeOut()
          this.setTransform(this.t.m + this.t.sx)
          var x = this.getTransform()
          x += this.t.m > 0 ? this._width * 0.3 : this._width * -0.3
          this.index = Math.round(x / this._width) * -1
          this.wh('touch')
        }
      },
      setTransform(num) {
        this.dom.transform = `translate3d(${num}px, 0px, 0px)`
        this.dom['-webkit-transform'] = `translate3d(${num}px, 0px, 0px)`
        this.dom['-ms-transform'] = `translate3d(${num}px, 0px, 0px)`
      },
      getTransform() { //这是干啥的,获取移动的距离
        var x = this.dom.transform || this.dom['-webkit-transform'] || this.dom['-ms-transform'];
        x = x.substring(12)
        x = x.match(/(\S*)px/)[1]
        return Number(x)
      },
      fn(e) {
        e.preventDefault()
      },
      prevSlide() {
        this.clearTimeOut()
        this.index--
        this.wh()
      },
      nextSlide() {
        this.clearTimeOut()
        this.index++
        this.wh()
      },
      slideTo(index) {
        this.clearTimeOut()
        this.index = index + 1
        this.wh()
      },
      wh(type) {
        this.slideing = false
        this.dom.transition = type == 'touch' ? '250ms' : this.duration + 'ms'
        this.setTransform(this.index * -1 * this._width)
        this.t.m = 0
        this.t.s = -1 //保证下次重新赋值
        if (this.autoPlay) {
          this.setTime()
        }
        var timeDuration = type == 'touch' ? '250' : this.duration
        setTimeout(() => {
          this.dom.transition = '0s'
          if (this.index >= this.slidesLength + 1) {
            this.index = 1
            this.setTransform(this.index * -1 * this._width)
          }
          if (this.index <= 0) {
            this.index = this.slidesLength
            this.setTransform(this.index * -1 * this._width)
          }
          this.$emit('transtionend', this.index - 1)
          this.auto = true
          this.slideing = true
        }, timeDuration)
      },
      setTime() {
        this.timer1 = window.setTimeout(() => {
          if (this.auto) {
            this.index++
            this.wh()
          } else {
            window.clearTimeout(this.timer1)
          }
        }, this.interval)
      },

      /*克隆了滑块 并且计算出了宽度*/
      starDom() {
        //获取滑块
        var SlideDom = document.querySelector('.' + this.className).getElementsByClassName('wh_slide')
        // 滑块个数
        this.slidesLength = SlideDom.length

        if (this.slidesLength > 1) {
          var cloneDom1 = SlideDom[0].cloneNode(true) //向最后append
          var cloneDom2 = SlideDom[this.slidesLength - 1].cloneNode(true) //向最前append
          document.querySelector('.' + this.className).insertBefore(cloneDom2, SlideDom[0])
          document.querySelector('.' + this.className).appendChild(cloneDom1)
          this._width = document.querySelector('.' + this.className).offsetWidth
          console.log(this._width,'uld的宽度');
          this.dom = document.querySelector('.' + this.className).style
        }
      },
      clearTimeOut() {
        this.auto = false
        window.clearTimeout(this.timer1)
      }
    },
    mounted() {
      this.className = `wh_swiper_${Math.random().toFixed(3) * 1000}`
      setTimeout(() => {
        //克隆dom
        this.starDom()
        this.dom.transform = `translate3d(${this._width * -1}px, 0px, 0px)`
        this.dom['-webkit-transform'] = `translate3d(${this._width * -1}px, 0px, 0px)`
        this.dom['-ms-transform'] = `translate3d(${this._width * -1}px, 0px, 0px)`
        if (this.autoPlay) {
          this.setTime()
        }
      }, 50)

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .wh_content {
    position: relative;
    z-index: 1;
    overflow: hidden;
    width: 100%;
  }

  .wh_swiper {
    width: 100%;
    display: flex;
    -moz-transition-duration: 0s linear;
    -webkit-transition-duration: 0s linear;
    -o-transition-duration: 0s linear;
    transition-duration: 0s linear;
  }

  .wh_slide {
    width: 100%;
    -ms-flex-negative: 0;
    flex-shrink: 0;
    z-index: 10;
    min-height: 100px;
    background-color: red;
  }

  .wh_slide img {
    display: block;
  }


  .wh_show_bgcolor {
    background: #0fc37c;
  }
</style>
