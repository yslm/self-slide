<template>
  <div class="hello">
    <p>轮播</p>

    <ul ref="ul" class="ul" @touchstart="startTouch" @touchmove="moveTouch" @touchend="endTouch">
      <li>00000001</li>
      <li>00000002</li>
      <li>00000003</li>
      <li>00000004</li>
      <li>00000005</li>
      <li>00000006</li>
      <li>00000007</li>
      <li>00000008</li>
      <li>00000009</li>
      <li>00000010</li>
    </ul>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      currentIndex:0, //当前的编号
      moveX:0, //滑动的距离
      currentLeft:0, //左滑的距离
      slideing:false,
      startPlace:0 //刚接触的位置
    }
  },
  methods:{
    startTouch(e){
      let ul=document.querySelector('.ul');
      this.currentLeft=ul.scrollLeft;
      this.moveX=0;
      this.slideing=true;
      if(this.slideing){
        this.startPlace=e.touches[0].clientX;

      }
    },
    moveTouch(e){
      if(this.slideing){
        this.moveX=e.touches[0].clientX-this.startPlace;
      }
      let ul=document.querySelector('.ul');
      // ul.scrollLeft=this.currentLeft-this.moveX;
      ul.scrollLeft=this.currentLeft-this.moveX;

    },
    endTouch(){
      let ul=document.querySelector('.ul');
      this.currentLeft=ul.scrollLeft;
      let leng=document.querySelector('.ul').offsetWidth;
      if(this.moveX>0){
        //右划
        console.log('右',leng);
        //只要大于1/4,就滑动到下一屏
        if(this.moveX>100){
          if(this.currentIndex>=9){
            this.currentIndex=9;
            return
          }
            this.currentIndex+=1;
          console.log(this.currentIndex,'优化');
          this.scrollLeft(this.currentLeft,this.currentLeft-(leng-this.moveX))
        }else {
          //吸附回去
          this.scrollLeft(this.currentLeft,this.currentLeft+(leng-this.moveX))
        }
        // ul.scrollLeft=this.currentLeft-(leng-this.moveX);
      }else{
        //左滑
        if(this.moveX<-100){
          if(this.currentIndex<=0){
            this.currentIndex=0;
            return
          }
          console.log('左',this.currentIndex);
          this.currentIndex-=1;
          console.log(this.currentIndex);
          this.scrollLeft(this.currentLeft,this.currentLeft+(leng+this.moveX))
        }else {
          //吸附回去
          console.log('左吸附');
          this.scrollLeft(this.currentLeft,this.currentLeft+this.moveX)
        }
      }

    },

  /*  scroll(scrollLeft){
      const scrollOffset = scrollLeft;
      this.scrollLeft(this.currentLeft,scrollOffset)
    },*/

    scrollLeft(from, to){
      const difference = Math.abs(from - to);
      const step =  Math.ceil(difference / 600 * 50);
      scroll=(start, end, step)=>{
        if (start === end) {
          return;
        }

        var d = (start + step > end) ? end : start + step;
        if (start > end) {
          d = (start - step < end) ? end : start - step;
        }
        this.$refs.ul.scrollLeft = d;
        this.currentLeft = d;
        window.requestAnimationFrame(function (number) {
          scroll(d, end, step)
        } );
      };

      scroll(from, to, step);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">


  .ul{
    box-sizing: content-box;
    width: 300px;
    height: 100px;
    direction: rtl;
    white-space: nowrap;
    overflow: hidden;
    /*overflow-x: auto;*/
    /*overflow-y: hidden;*/
    -webkit-overflow-scrolling: touch;
    /*border: 1px solid #000;*/
    font-size: 0;
    margin: 0;
    padding: 0;
  }
  .ul>li{
    width: 300px;
    list-style: none;
    height: 100px;
    line-height: 100px;
    font-size: .28rem;
    display: inline-block;
    background-color: red;
  }
</style>
