
<script>
import { ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core';
import { useRouter } from 'vue-router';
export default {
  props:["scrollMove"],
  setup(props){
    const menuIsOpen = ref(false);
    const router = useRouter();
    const isOpenFn = () => {
      menuIsOpen.value = !menuIsOpen.value;
    }

    onMounted(() => {
      
      })

    const historyBtn = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      const timeTop = setInterval(() => {
        if(props.scrollMove[0].list.history > top){
          document.body.scrollTop = document.documentElement.scrollTop = top += 70
          if (top > props.scrollMove[0].list.history + 100) {
            clearInterval(timeTop)
          }
        }else{
          document.body.scrollTop = document.documentElement.scrollTop = top -= 70
          if (top <= props.scrollMove[0].list.history) {
            clearInterval(timeTop)
          }
        }
      }, 10)
      isOpenFn();
    };

    const menuBtn = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      const timeTop = setInterval(() => {
        if(props.scrollMove[0].list.menu > top){
          document.body.scrollTop = document.documentElement.scrollTop = top += 100
          if (top > props.scrollMove[0].list.menu) {
            clearInterval(timeTop)
          }
        }else{
          document.body.scrollTop = document.documentElement.scrollTop = top -= 100
          if (top <= props.scrollMove[0].list.menu) {
            clearInterval(timeTop)
          }
        }
      }, 10)
      isOpenFn();
    };

    const inforBtn = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      const timeTop = setInterval(() => {
        if(props.scrollMove[0].list.infor > top){
          document.body.scrollTop = document.documentElement.scrollTop = top += 100
          if (top > props.scrollMove[0].list.infor) {
            clearInterval(timeTop)
          }
        }else{
          document.body.scrollTop = document.documentElement.scrollTop = top -= 100
          if (top <= props.scrollMove[0].list.infor) {
            clearInterval(timeTop)
          }
        }
      }, 10)
      isOpenFn();
    };


    const newsBtn = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      const timeTop = setInterval(() => {
        if(props.scrollMove[0].list.news > top){
          document.body.scrollTop = document.documentElement.scrollTop = top += 50
          if (top > props.scrollMove[0].list.news) {
            clearInterval(timeTop)
          }
        }else{
          document.body.scrollTop = document.documentElement.scrollTop = top -= 50
          if (top <= props.scrollMove[0].list.news) {
            clearInterval(timeTop)
          }
        }
      }, 10)
      isOpenFn();
    }

    return{
      menuIsOpen,
      isOpenFn,
      props,
      historyBtn,
      menuBtn,
      inforBtn,
      newsBtn
    }
  }
  }
</script>

<template>
<article class="menu">
  <div :class="['menuBtn', {menuBtn_close: menuIsOpen}]" @click="isOpenFn">
    <a class="menuTrigger">
      <span></span>
    </a>
    <div>
      <span>Menu</span>
      <span v-show="menuIsOpen">Close</span>
    </div>
  </div>
  <div class="menuList">
    <ul>
      <li>
        <a href=""></a>
        <a href=""></a>
      </li>
      <li>
        <a href=""></a> 
        <a href=""></a>
      </li>
      <li>
        <a href=""></a>
        <a href=""></a>
      </li>
      <li>
        <a href=""></a>
        <a href=""></a>
      </li>
    </ul>
  </div>
</article>
<article :class="['menu_list', {isOpen: menuIsOpen}]" >
  <ul>
    <li @click="newsBtn">
      <h1>NEWS</h1>
      <p>ニュース</p>
    </li>
    <li @click="historyBtn">
      <h1>Concept</h1>
      <p>コンセプト</p>
    </li>
    <li @click="menuBtn">
      <h1>Menu</h1>
      <p>メニュー</p>
    </li>
    <li @click="inforBtn">
      <h1>Shop Info</h1>
      <p>ショップインフォ</p>
    </li>
  </ul>
</article>
</template>

<style lang="scss" scoped>
  $pad: 769px;
  $pad-title-0: 46px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;
  $mobile: 376px;
  $mobile-title-2: 20px;
  article.menu{
    margin-top: 150%;
    >.menuBtn{
      display: flex;
      align-items: center;
      flex-direction: column;
      cursor: pointer;
      >a.menuTrigger{
          z-index: 2;
        >span{
          position: relative;
          height: 5px;
          display: block;
          width: 18px;
          margin: 8px;
          border-radius: 2.5px;
          background-color: #BE9C6D;
          &::before{
            content: "";
            position: absolute;
            bottom: -20px;
            left: -8px;
            height: 5px;
            width: 18px;
            margin: 8px;
            border-radius: 2.5px;
            background-color: #BE9C6D;
          }
          &::after{
            content: "";
            position: absolute;
            top: -20px;
            left: -8px;
            height: 5px;
            width: 18px;
            margin: 8px;
            border-radius: 2.5px;
            background-color: #BE9C6D;
          }
        } 
      }
      &.menuBtn_close{
        >a.menuTrigger{
          >span{
            background-color: transparent;
            &::before{
              background-color: #fff;
              transform: rotate(45deg);
              bottom: -8px;
              left: -15px;
              transition: all .3s;
              height: 5px;
              width: 36px;
            }
            &::after{
              background-color: #fff;
              transform: rotate(-45deg);
              top: -8px;
              left: -15px;
              transition: all .3s;
              height: 5px;
              width: 36px;
            }
          }
        }
        >span{
          color: #fff;
          z-index: 3;
        }
      }
      >div{
        margin-top: 15px;
        position: relative;
        >span{
          font-size: 16px;
          color: #BE9C6D;
          &:nth-of-type(2){
            position: absolute;
            top: 5px;
            left: 0;
            color: #fff;
            z-index: 3;
          }
        }
      }
    }
    >.menuList{
      >ul{
        list-style: none;
      }
    }
  }
  article.menu_list{
    position: fixed;
    top: 0;
    right: 0;
    background-color: #BE9C6D;
    width: 50vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0;
    transition: opacity .3s;
    z-index: -1;
    @media screen and (max-width: $pad){
      width: 100vw;
    }
    &.isOpen{
      opacity: 1;
      z-index: 1;
    }
    >ul{
      list-style: none;
      color: #fff;
      text-align: center;
      >li{
        padding: 10% 5%;
        cursor: pointer;
        width: 300px;
        position: relative;
        &:hover{
          &::before{
            transition: height .3s;
            height: 60%;
          }
        }
        &::before{
          content: "";
          position: absolute;
          left: 0%;
          top: 25%;
          height: 0%;
          border-left: 3px solid #fff;
        }
        >h1{
          font-size: 36px;
          margin-bottom: 5%;
        }
        >p{
          font-size: 16px;
        }
      }
    }
  }

</style>