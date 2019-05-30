<template>
  <div class="wrap">
   <!-- <ul class="ul" ref="ul" @touchstart="startTouch" @touchmove="moveTouch" @touchend="endTouch">
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
    </ul>-->

    <div class="card-box" @touchstart="startTouch" @touchmove="moveTouch" @touchend="endTouch">
      <ul class="ul" ref="ul">
        <li :class="{'select':index===idx}" v-for="(item,idx) in 10">
          <p>{{item}}---{{index}}</p>
        </li>
      </ul>
   <!--   <button class="swiper-btn-left" v-show="chooseCard<badgeImgUrl.length-1" @click="swiperLeft"></button>
      <button class="swiper-btn-right" v-show="chooseCard>0" @click="swiperRight"></button>-->


    </div>
  </div>

</template>

<script>

  export default {
    data(){
      return{
        startX:0,
        moveX:0,
        translateX:0,
        index:0,
        wrapDom:null,
        slideLength:1,//子元素的个数,默认是1
        _width:0 //子元素的宽度
      }
    },
    methods:{
      //初始化
      init(){
        this.wrapDom=this.$refs.ul;
        this.slideLength=this.wrapDom.children.length;
        this._width=this.wrapDom.children[0].offsetWidth;
        console.log(this._width);
        this.wrapDom.style.transitionDuration='0s';
        this.wrapDom.style.transform='translate(0px)';
      },
      startTouch(e){
        this.startX=e.touches[0].clientX;
        this.translateX=this.getTransform();
      },
      moveTouch(e){
        this.moveX=e.touches[0].clientX-this.startX;
        this.wrapDom.style.transform='translate('+(this.translateX+this.moveX)+'px)';
      },
      endTouch(){
        this.wrapDom.style.transitionDuration='250ms';
        if(this.moveX<0){
          //向左
          if(Math.abs(this.moveX)>50){
            //下一页
            console.log('左');
            if(this.index<=0){
              this.index=0;
              this.wrapDom.style.transform='translate('+(this._width*this.index)+'px)';
              return;
            }
            this.index--;
            console.log('右',this.index);
          }else {
            //弹回
            console.log('左弹回');
          }
          this.wrapDom.style.transform='translate('+(this._width*this.index)+'px)';

        }else{
          //向右
          if(Math.abs(this.moveX)>50){
            //下一页
            if(this.index>=this.slideLength-1){
              this.index=this.slideLength-1;
              this.wrapDom.style.transform='translate('+(this._width*this.index)+'px)';
              return;
            }
            this.index++;
            console.log(this.index);

          }else {
            //弹回
            console.log('右弹回');
          }
          this.wrapDom.style.transform='translate('+(this._width*this.index)+'px)';

        }
      },
      /*获取移动的属性*/
      getTransform(){
        var x = this.wrapDom.style.transform;
        x = x.substring(10)
        x = x.match(/(\S*)px/)[1]
        return Number(x)
      }
    },
    mounted(){
      this.init()

    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  *{
    margin: 0;
    padding: 0;
  }
  .card-box{
    position: relative;
    width: 100%;
    height: 2rem;
    overflow: hidden;
    background-color: #fffdb2;
    margin: 0.2rem auto 0;
    .ul{
      min-width: 100%;
      position: absolute;
      right: 0.9375rem;
      top: 0;
      display: flex;
      flex-direction: row-reverse;
      align-items: center;
      transition: all 0.5s;
      font-size: 0;
      li{
        position: relative;
        width: 1.875rem;
        height: 2rem;
        text-align: center;
        transition: all 0.5s;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        /*background-color: red;*/
        color: red;
        background-color: green;
        font-size: 20px;
        &>img{
          opacity: 0.4;
          width: 1.2rem;
          height: 1.2rem;
          transition:  all 0.3s;
        }
      }
      .select{
        color: indigo;
        &>img{
          opacity: 1;
          transform: scale(1.2);
          width: 1.2rem;
          /*height: 1.92rem;*/
        }

        .get-reward,.got-reward{
          p{
            top: 0.8rem;
            color: #029392;
            font-size: 0.22rem;
            line-height: 0.3rem;
          }
        }
        .get-reward{
          button{
            bottom: 0.16rem;
            left: 0.26rem;
            width: 0.98rem;
            height: 0.28rem;
          }
        }
      }
    }


  }


</style>
