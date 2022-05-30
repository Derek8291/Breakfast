
<script>
import HeaderNav from "@/components/Header_nav.vue"
import { headImg } from "@/lib/data.js"
import { reactive, ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core'
export default {
  props:["yScrollData"],
  components: {
    HeaderNav,
    headImg
  },
  setup(props) {
    const newArr = reactive({list:[]});
    const scrollMove = reactive([]);
    const imgPage = ref(0);
    let timer = null;
    newArr.list = headImg;
  
    const timeGo = () => {
      timer = setInterval(() => {
        imgPage.value++;
        if(imgPage.value > newArr.list.length - 1) imgPage.value = 0;
      }, 5000)
    };

    timeGo();

    const pageChange = (index) => {
      imgPage.value = index;
      clearInterval(timer);
      // setTimeout(() => {
      //   timeGo()
      // }, 3000);
    };


    onMounted(() => {
      scrollMove.push(props.yScrollData);
    })


    return{
      newArr,
      imgPage,
      pageChange,
      props,
      scrollMove
      
    }
  }
  }
</script>

<template>
  <header>
      <transition-group tag="div" name="fade" class="header_left">
          <div class="main_img" v-show="index === imgPage" v-for="(img, index) of newArr.list" :key="index-1">
            <img :src="img.src" alt="" >
          </div>
      </transition-group>
    <div class="header_right">
      <ul>
        <li>
          <HeaderNav :scrollMove="scrollMove"/>
        </li>
        <li>
          <img src="@/assets/header/mainttl.png" alt="">
          <img src="@/assets/header/maincatch.png" alt="">
        </li>
        <li>
          <img src="@/assets/header/logo-tate.png" alt="">
        </li>
        <li>
          <span v-for="(num, index) in newArr.list.length" :key="num-1" @click="pageChange(num-1)" :class="[{toBig: index === imgPage}]"></span>
        </li>
      </ul>
    <!-- <HeaderNav /> -->
    </div>
  </header>
</template>

<style lang="scss" scoped>
  $pro: 1025px;
  $pad: 769px;
  $pad-title-0: 46px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;
  $mobile: 376px;
  $mobile-title-2: 20px;
  
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
  .fade-enter-to,
  .fade-leave-from {
    opacity: 1;
  }


  header{
    height: calc(100vh - 120px);
    min-height: 700px;
    width: 100%;
    display: flex;
    // border: 1px solid red;
    >.header_left{
      width: calc(100% - 250px);
      height: 100%;
      position: relative;
      @media screen and (max-width: $pro){
        width: calc(100% - 150px);
      }
      @media screen and (max-width: $pad){
        width: calc(100% - 100px);
      }
      @media screen and (max-width: $mobile){
        width: calc(100% - 80px);
      }
      >ul.icon{
        position: fixed;
        top: 30px;
        left: 20px;
        list-style: none;
        display: flex;
        background-color: transparent;
        >li{
          margin-right: 15px;
          background-color: transparent;
          >a{
            background-color: transparent;
            >i{
              background-color: transparent;
              color: #004c34;
              font-size: 20px;
            }
          }
        }
      }
      >div.main_img{
        height: 100%;
        width: 100%;
        position: absolute;
        overflow: hidden;
        >img{
          top: 0;
          left: 0;
          height: 100%;
          width: 100%;
          object-fit: cover;
          @media screen and (max-width: $mobile){
            
          }
        }
      }
    }
    .header_right{
      width: 250px;
      height: 100%;
      // border: 1px solid green;
      display: flex;
      justify-content: center;
      @media screen and (max-width: $pro){
        width: 150px;
      }
      @media screen and (max-width: $pad){
        width: 100px;
      }
      @media screen and (max-width: $mobile){
        width: 80px;
      }
      >ul{
        list-style: none;
        display: flex;
        flex-direction: column;
        justify-content:space-between;
        align-items:center;
        height: 100%;
        >li{
          // border: 1px solid blue;
          width: auto;
          height: auto;
          display:flex;
          align-items: center;
          justify-content: center;
        }
        >li:nth-of-type(2){
          display: flex;
          justify-content: space-between;
          align-items: flex-start;
          width: 100px;
          @media screen and (max-width: $pad){
            width: 70px;
          }
          >img:nth-of-type(1){
            width: 48px;
            @media screen and (max-width: $pad){
            width: 36px;
            }
          }
          >img:nth-of-type(2){
            width: 18px;
            @media screen and (max-width: $pad){
            width: 15px;
            }
          }
        }
        >li:nth-of-type(3){
          >img{
            width: 110px;
            @media screen and (max-width: $pad){
              width: 70px;
            }
            @media screen and (max-width: $mobile){
              width: 60px;
            }
          }
        }
        >li:nth-of-type(4){
          display: flex;
          justify-content: space-between;
          width: 110px;
          @media screen and (max-width: $pad){
            width: 70px;
            }
          @media screen and (max-width: $mobile){
            width: 70px;
            }
          >span{
            display: block;
            width: 8px;
            height: 8px;
            border-radius: 2px;
            transform: rotate(45deg);
            background-color: #be9c6d;
            z-index: 0;
            cursor: pointer;
            &.toBig{
              width: 12px;
              height: 12px;
              border-radius: 3px;
              background-color: #004c34;
            }
          }
        }
      }
    }
  }
</style>