<template>
  <div>
    <!--用过监听getindex，拿到从子组件传递过来的数据-->
    <logo-select  @getindex='getIndex'></logo-select>
    <div class="search-input">
      <!--keyup是按下获得搜索引擎数据，keydown回车是直接搜索网页，down和up是上移下移-->
      <input type="text"
             v-model="keyword"
             @keyup="get($event)"
             @keydown.enter="search"
             @keydown.down="selectDown"
             @keydown.up.prevent="selectUp"
      >
      <span class="search-reset" @click="clearInput">&times;</span>
      <button class="search-btn" @click="search">搜一下</button>
      <div class="search-select">
        <transition-group name="itemfade" tag="ul" mode="in-out">
          <li class="search-select-option search-select-list"
              v-for="(value,index) in myData"
              :class="{ selectback : index === iNow }"
              @mouseover="selectHover(index)"
              @click="selectClick(value)"
              :key="value"
          >
            {{value}}
          </li>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script type="text/javascript">
  import logoSelect from './select';

  export default {
    //注册组件
    components: {
      logoSelect
    },
    data: function() {
      return {
        myData: [],//存数据
        keyword: '',//绑定input的值
        iNow: -1,//用来判断class的上下移动
        searchIndex: 0,//拿到子组件选择的index，然后用不同的搜索引擎
        logoData: [{
          name: '百度搜索',
          searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
        }, {
          name: 'Google搜索',
          searchSrc: 'https://www.google.com.hk/#safe=strict&q='
        }, {
          name: '360搜索',
          searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
        }]
      }
    },
    methods: {
      getIndex: function(index) {
        this.searchIndex = index;
      },
      get (ev) {
        //如果按的键是上或下，不搜索
        if(ev.keyCode === 38 || ev.keyCode === 40){
          return;
        }
        this.$http.jsonp('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su',{params:{
          wd: this.keyword
        },jsonp: 'cb'}).then(function (res) {
          this.myData = res.data.s;
        });
      },
      search () {
        //打开网页
        window.open(this.logoData[this.searchIndex].searchSrc+this.keyword);
      },
      selectDown () {
        //向下
        this.iNow++;
        if(this.iNow === this.myData.length){
          this.iNow = 0;
        }
        this.keyword = this.myData[this.iNow];
      },
      selectUp () {
        this.iNow--;
        if(this.iNow === -1){
          this.iNow = this.myData.length - 1;
        }
        this.keyword = this.myData[this.iNow];
      },
      selectHover (index) {
        this.iNow = index;
      },
      selectClick (value) {
        this.keyword = value;
        this.search();
      },
      clearInput () {
        this.keyword = '';
        this.myData = [];
        this.iNow = -1;
      }
    }
  }
</script>

<style type="text/css">
  .search-input {
    height: 45px;
    width: 600px;
    margin: 0 auto;
    margin-top: 10px;
    position: relative;
  }

  .search-input input {
    border: 1px solid #e4e4e4;
    box-sizing: border-box;
    width: 500px;
    height: 45px;
    font-size: 18px;
    float: left;
    padding-left: 10px;
    padding-right: 10px;
    overflow: hidden;
  }

  .search-btn {
    height: 45px;
    width: 100px;
    border: 1px solid mediumseagreen;
    background-color: mediumseagreen;
    color: white;
    font-size: 16px;
    font-weight: bold;
    float: left;
  }

  .search-btn {
    cursor: pointer
  }

  .search-select {
    position: absolute;
    top: 45px;
    width: 500px;
    box-sizing: border-box;
    z-index: 999;
  }

  .search-select li {
    border: 1px solid #d4d4d4;
    border-top: none;
    border-bottom: none;
    background-color: #fff;
    width: 100%
  }

  .search-select-option {
    box-sizing: border-box;
    padding: 7px 10px;
  }

  .selectback {
    background-color: #eee !important;
    cursor: pointer
  }

  input::-ms-clear {
    display: none
  }

  .search-reset {
    width: 21px;
    height: 21px;
    position: absolute;
    display: block;
    line-height: 21px;
    text-align: center;
    cursor: pointer;
    font-size: 20px;
    right: 110px;
    top: 12px
  }

  .search-select-list {
    transition: all 0.3s
  }

  .itemfade-enter,
  .itemfade-leave-active {
    opacity: 0;
  }

  .itemfade-leave-active {
    position: absolute;
  }

  .selectback {
    background-color: #eee !important;
    cursor: pointer
  }
  .search-select ul{margin:0;text-align: left; }
</style>
