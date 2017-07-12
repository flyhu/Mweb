<template>
  <div id="wrapper">  
  <loading v-show="showLoading"></loading>    
          <div class="scrollbar" @touchstart="startDrag" @touchmove="startMove">
              <div class="nav" ref="nav">
                <ul :style="{width: navWidth + 'px' }">
                <li class="active">推荐</li>
                  <li v-for="(item, index) in foods">{{item.name}}</li>
                </ul>
              </div>
          </div>

        <div class="main">
          <header>
              <mt-swipe :auto="4000">
                <mt-swipe-item v-for="img in images">
                <img :src="img.src" alt="">
                </mt-swipe-item>
              </mt-swipe>
          </header>
            <div style="height:1000px;"><p>gdfgdfg</p><p>gdfgdfg</p><p>gdfgdfg</p><p>gdfgdfg</p></div>
        </div>
  </div>
</template>

<script>
  //1rem=100px;
 document.documentElement.style.fontSize=
 document.documentElement.clientWidth/375*100+'px';

import loading from './components/common/loading' 
import './assets/css/reset.css';
import data from './assets/json/data.json';

  export default {
    name: 'app',
    components: {
      loading
    },
    data() {
      return { 
        navWidth:'',//导航宽度
        startPos: { x: 0, y: 0 },
        showLoading: true,
        images:[{src:'https://css.meishij.net/zt/images/ad/chirou.jpg',link:'https://m.meishij.net'},{src:'https://css.meishij.net/zt/images/ad/meishiriji.jpg',link:'https://m.meishij.net'},{src:'https://css.meishij.net/zt/images/ad/hongbei.jpg',link:'https://m.meishij.net'},{src:'https://css.meishij.net/zt/images/ad/huixiang.jpg',link:'https://m.meishij.net'}],
        foods:[]
      };
    },
    created() {     
      this.foods = data.goods;
      this.$nextTick(() => {
        this._navWidth();
      });
    },
    computed: {
     
    },
    methods:{
       _navWidth() { //计算导航宽度
        var li = this.$refs.nav.getElementsByTagName("li");
        var Width = 0;
        for (var i = 0; i < li.length; i++) {
          Width +=li[i].clientWidth+16
        }
        this.navWidth=Width;
      },
      startDrag: function (e) {
        e = e.changedTouches ? e.changedTouches[0] : e;
        this.startPos.y=e.pageY;
        this.startPos.x=e.pageX;
      },
      startMove: function (event) { //阻止Nav纵向滑动
        var e = event.changedTouches ? event.changedTouches[0] : event;
        var endPos = {x:e.pageX - this.startPos.x, y:e.pageY - this.startPos.y};
        
        if (Math.abs(endPos.x) > Math.abs(endPos.y) && endPos.x > 0 ) {
                console.log("左滑");
            }
            else if (Math.abs(endPos.x) > Math.abs(endPos.y) && endPos.x < 0) {
                console.log("右滑");
            }
            else if (Math.abs(endPos.y) > Math.abs(endPos.x) && endPos.y > 0) {
                console.log("下滑");
            }
            else if (Math.abs(endPos.y) > Math.abs(endPos.x) &&  endPos.y < 0) {
                console.log("上滑");
            }
            else{
                console.log("touch");
            }

        var isScrolling = Math.abs(endPos.x) > Math.abs(endPos.y) ? 1:0;
        if(isScrolling===0){
          //console.log('纵向')
          event.preventDefault()
        }else {
            //console.log('横向')
        }

    },
    }
   

  }


</script>
<style>
html, body {
/*    background: #f5f5f5;*/
    background-color: #fff;
    overflow-x: hidden;
    height: 100%;
}
.wrapper{
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.mint-swipe, .mint-swipe-items-wrap {
    width: 100%;
    height: 2rem;
}
.mint-swipe-item{
  width: 100%;
}
.mint-swipe-item img{
  width: 100%;
  min-height: 2rem;
}
.mint-swipe-indicator{
  height: 6px;
  width: 6px;
  border-radius: 50%;
}
.mint-swipe-indicator.is-active{
    opacity: 1;
    background: #ff4c39;
}
.scrollbar{
  height: 45px;
  overflow: hidden;
}
.nav{
  position: relative;
  height: 60px;
  background-color: #fff;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;/*移动端*/
}
.scrollbar::-webkit-scrollbar{display:none}
.nav ul{ 
  height: 45px;
  line-height: 45px;
  padding: 0 8px;
  vertical-align: middle;
}
.nav li{
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  display: inline-block;
  height: 40px;
  margin: 0 8px;
  text-align: center;
}
.nav li.active{
  border-bottom: 2px solid #ff4c39;
}

</style>
