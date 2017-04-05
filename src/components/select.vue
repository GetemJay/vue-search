<template>
  <div class="main-logo">
    <img :src="img[logoIndex].imgSrc">
    <span class="logoList-arrow" @click="listToggle"></span>
    <transition name="logofade">
      <ul class="logoList" v-show="listShow">
        <li class="logoItem"
            v-for="(item,index) in img"
            @click="logoChange(index)"
            @mouseover="listHover(index)"
            :class="{selectback:index==iNow}"
        >
          <img :src="item.imgSrc">
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'select',
  data () {
    return {
      iNow: -1,
      logoIndex: 0,
      listShow: false,
      img: [{
        imgSrc: require('../assets/baidu_logo.png')
      },{
        imgSrc: require('../assets/google_logo.png')
      },{
        imgSrc: require('../assets/360_logo.png')
      }]
    }
  },
  methods: {
    listToggle () {
      this.listShow = !this.listShow
    },
    logoChange (index) {
      this.logoIndex = index;
      this.listShow = !this.listShow;
      //把选择的搜索引擎index传出去
      this.$emit('getindex',index);
    },
    listHover (index) {
      this.iNow = index
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  ul{list-style: none;padding: 0;margin: 0}
  .main-logo {
    width: 600px;
    height: 140px;
    position: relative;
  }

  .main-logo img {
    display: block;
    margin: 0 auto;
  }

  .logoList-arrow {
    position: absolute;
    width: 0;
    height: 0;
    border: 8px solid;
    border-color: #000 transparent transparent transparent;
    right: 100px;
    top: 66px;
    cursor: pointer
  }

  .logoList {
    position: absolute;
    top: 100%;
    width: 200px;
    left: 50%;
    margin-left: -100px;
    z-index: 999999;
    border: 1px solid #d4d4d4
  }

  .logoList li {
    width: 100%;
    height: 80px;
    background-color: #fefefe;
    line-height: 80px;
    padding-top: 1px;
  }

  .logoList li img {
    width: 100%;
    margin-top: 10px;
  }

  .logofade-enter-active,
  .logofade-leave-active {
    transition: all .5s;
  }

  .logofade-enter,
  .logofade-leave-active {
    opacity: 0;
    transform: translateY(20px);
  }
  .selectback{background-color: #eee !important;cursor: pointer}
</style>
