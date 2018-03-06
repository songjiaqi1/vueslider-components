<template>
  <div>
    <slider :img="img" :width="width" :height="height"></slider>
  </div>

</template>

<script>
//  import swiper from 'swiper'

  import slider from './slider1.vue'


  export default {

    data(){
      return {
        img:[{src:require('../assets/slideShow/pic1.jpg'),name:'hehe1'},
          {src:require('../assets/slideShow/pic2.jpg'),name:'hehe2'},
          {src:require('../assets/slideShow/pic3.jpg'),name:'hehe3'},
          {src:require('../assets/slideShow/pic4.jpg'),name:'hehe4'}
        ],
        width:460,
        height:250
      }
    },

    components:{
      slider:slider
    }
  }
</script>
<style >

</style>
