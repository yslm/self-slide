<template>
  <div class="wrap">
  <ul class="ul">
    <li>0000001</li>
    <li>0000002</li>
    <li>0000003</li>
    <li>0000004</li>
    <li>0000005</li>
    <li>0000006</li>
    <li>0000007</li>
    <li>0000008</li>
    <li>0000009</li>
    <li>0000010</li>
  </ul>

  </div>
</template>

<script>
    export default {
        name: "test-swiper",
      data(){
          return{

            slidesLength: 1, //滑块的个数，默认是一个
            _width: 0,   // ul的宽度，和外盒子宽度一样
            t: {
              sx: 0, //初始移动的距离
              s: 0, //touchu的初始位置  touches[0].clientX
              m: 0,  //滑动的距离 也就是moveX
              e: 0
            }
          }
      },
      methods:{
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
        }
      }
    }
</script>

<style scoped>
  .wrap{
    position: relative;
    z-index: 1;
    overflow: hidden;
    width: 100%;
    margin-top: 1rem;
  }
  ul{
    margin: 0;
    padding: 0;
    width: 100%;
    display: flex;
    transition: all 2s;
    /*transition-duration: 0s;*/
  }
  ul>li{
    list-style: none;
    width: 100%;
    /*-ms-flex-negative: 0;*/
    flex-shrink: 0; /*为了控制不缩小*/
    z-index: 10;
    min-height: 100px;
    background-color: red;
  }
</style>
