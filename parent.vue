<template>
  <div class="box">
    <div @mouseenter="endInterval" @mouseleave="startInterval" class="container">
      <div :style="{width:wrap_width+'px',height:wrap_height+'px',left:offset_left+'px'}" class="slider-wrap">
        <div class='img' v-for="item in slider_des">
          <img :src="item.src" alt="">
        </div>
      </div>
      <div class="bottom">
        <ul class="pointContainer">
          <li @click="changeIndex(index)" :class="{point:true,active:nowIndex===index}" v-for="(point,index) in length"></li>
        </ul>
      </div>
      <div @click="pre" class="click pre">&lt;</div>
      <div @click="next" class="click next">&gt;</div>
    </div>
  </div>
</template>

<script>
  export default {
    props:{
      img:{
        type:Array,
        default:[]
      },
      width:{
        type:Number,
        default:460
      },
      height:{
        type:Number,
        default:250
      }
    },

    mounted(){
      this.startInterval();
    },

    data(){
      console.log(this.width);
      return{
        length:new Array(this.img.length),
        nowIndex:0,
        wrap_width:this.width*(this.img.length+2),
        wrap_height:this.height,
        offset_left:-this.width,
        isTransition:true,
        timer:null,
        animateFlag:true,
        timerAuto:null
      }
    },
    computed:{
      slider_des:function () {
        var arr=[];
        var arr1=arr.concat(this.img);
        arr1.push(this.img[0]);
        arr1.unshift(this.img[this.img.length-1]);
        return arr1;
      }
    },
    methods:{
      pre(){
        if(this.nowIndex===0){
          if(!this.animateFlag){
            clearInterval(this.timer);
            this.animateFlag=true;
            this.offset_left=-(this.length.length)*this.width;
          }
          this.animate(-this.width,0,function (that) {
            that.offset_left=-(that.length.length)*that.width;
          });
          this.nowIndex=this.img.length-1;
          return
        }else{
          if(!this.animateFlag){
            this.animateFlag=true;
            clearInterval(this.timer);
            this.offset_left=-(this.nowIndex*this.width);
          }
          this.animate(-(this.nowIndex+1)*this.width,-(this.nowIndex*this.width));
        }
        this.nowIndex-=1;
      },
      startInterval(){
        this.timerAuto=setInterval(this.next,2000);
      },
      endInterval(){
        clearInterval(this.timerAuto);
      },
      next(){
        if(this.nowIndex===this.length.length-1){
          if(!this.animateFlag){
            this.animateFlag=true;
            clearInterval(this.timer);
            this.offset_left=-this.width;
          }
          this.animate(-(this.length.length)*this.width,-(this.length.length+1)*this.width,function (that) {
            that.offset_left=-that.width;
          });
          this.nowIndex=0;
          return
        }else{
          if(!this.animateFlag){
            this.animateFlag=true;
            clearInterval(this.timer);
            this.offset_left=-(this.nowIndex+2)*this.width;
          }
          this.animate(-(this.nowIndex+1)*this.width,-(this.nowIndex+2)*this.width);
          this.nowIndex+=1;
        }
      },
      animate(start,end,fuc){
        var distance=end-start;
        var pre_dis=distance/50;
        var that=this;
        this.timer=setInterval(function () {
          that.animateFlag=false;
          if(Math.abs(end-that.offset_left)<=Math.abs(pre_dis)){
            that.offset_left=end;
            if(fuc){
              fuc(that);
            }
            that.animateFlag=true;
            clearInterval(that.timer);
            that.timer=null;
            return
          }
          that.offset_left+=pre_dis;
        },1);
      },
      changeIndex(index){
        clearInterval(this.timer);
        this.animate(-(this.nowIndex+1)*this.width,-(index+1)*this.width);
        this.nowIndex=index;
      }
    }
  }
</script>


<style scoped>
    *{
      margin: 0;
      list-style: none;
      /*height: 0;*/
    }
    .box{
      position: relative;
    }
    .container{
      width: 460px;
      height: 250px;
      margin: 0 auto;
      border: 1px solid #3bb4f2;
      overflow: hidden;
      left: 0;
      top: 0;
      position: absolute;
    }

    .slider-wrap{
      /*width: 2760px;*/
      /*height: 250px;*/
      position: absolute;
      /*left: -460px;*/
      /*overflow: hidden;*/
      top: 0;
    }

    .transition{
      transition: 0.5s;
    }

    .img{
      width: 460px;
      height: 250px;
      float: left;
      display: inline;
    }

    img{
      width: 460px;
      height: 250px;
      /*float: left;*/
    }

    .click{
      width: 20px;
      background-color: rgba(255,255,255,.3);
      color: aliceblue;
      font-weight: bold;
      position: absolute;
      height: 40px;
      line-height: 40px;
      margin-top: -20px;
      top: 50%;
      cursor: pointer;
    }

    .pre{
      left: 0;
    }
    .next{
      right: 0;
    }
.bottom{
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 20px;
  text-align: center;
}

  .pointContainer{
    height: 20px;
  }

  .point{
    display: inline-block;
    border: 5px solid #eeeeee;
    border-radius: 5px;
    line-height: 0;
    margin-right: 3px;
  }

  .active{
    border: 5px solid #42b983;
  }

</style>

