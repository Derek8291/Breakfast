
<script>
import { inforImg } from '@/lib/data.js';
import { reactive, ref } from '@vue/reactivity';
import { onMounted } from '@vue/runtime-core';


export default {
  emits: ["inforCallBack"],
  setup(props, { emit }) {
      const newArr = reactive({list: []});
      const imgPage = ref(0);
      const Yscroll = ref(null);
      let timer = null;
      newArr.list = inforImg;

      const imgChange = (e) => {
        imgPage.value = e;
        clearInterval(timer);
      }

      timer = setInterval(() => {
        if(imgPage.value > 1) imgPage.value = 0;
        imgPage.value++
      }, 5000);


      onMounted(() => {
        let inforYscroll = Yscroll._value.offsetTop;
        emit("inforCallBack", inforYscroll);
 

      });

 
      return{
        newArr,
        imgPage,
        imgChange,
        Yscroll
      }
    }
  }
</script>

<template>
  <article>
    <div class="title" ref="Yscroll">
      <h1 >Shop Info</h1>
    </div>
    <transition-group tag="ul" name="fade" class="inforImg" >
        <li class="main_img" v-for="(img, index) of newArr.list" :key="index" v-show="index === imgPage">
          <img :src="img.src" alt="">
        </li>
    </transition-group>
    <ul class="inforPoint">
      <li v-for="(item, index) in newArr.list" :key="index">
        <span @click="imgChange(index)" :class="[{toBig: index === imgPage}]"></span>
      </li>
    </ul>
  </article>
  <aside>
    <div class="inforTop">
      <div class="inforTop_left">
        <img src="../assets/information/logo-tate.png" alt="">
      </div>
      <div class="inforTop_right">
        <!-- <h2>wanna manna</h2>
        <h2>ワナマナ</h2> -->
        <table>
          <tbody>
            <tr>
              <th>OPEN</th>
              <td>
                8am - 8:30pm
                <p>
                  ※ただし、営業時間は変更となる場合がございます。
                <br>
                  最新情報はTopicsおよびSNSにて更新しておりますのでご確認くださいませ。
                </p>
              </td>
            </tr>
            <tr>
              <th>HOLIDAY</th>
              <td>日曜日(安息日)</td>
            </tr>
            <tr>
              <th>SEAT</th>
              <td>22席</td>
            </tr>
            <tr>
              <th>TEL</th>
              <td>06-6809-2128</td>
            </tr>
            <tr>
              <th>ADD</th>
              <td>〒530-0041 大阪府大阪市北区天神橋1丁目10-13</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="inforBottom">
      <img src="@/assets/information/map.png" alt="">
    </div>
  </aside>
  <br>
  <br>
  <br>
  <br>
  <br>

</template>

<style lang="scss" scoped>
  $pro: 1025px;
  $pad: 769px;
  $mobile: 376px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;

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

  article{
    // border: 1px solid red;
    padding-top: 100px;
    background-image: url("../assets/information/bg-moyo03.png");
    background-repeat: no-repeat;
    background-size: 30%;
    background-position: 100% 10%;
    width: 100%;
    // border: 1px solid red;
    >div.title{
    text-align: center;
    color: #004c34;
      >h1{
        font-size: 56px;
        font-weight: 400;
        @media screen and (max-width: 768px){
          font-size: $pad-title-1;
        }
      } 
    }
    >ul.inforImg{
      margin: 50px auto 10px;
      position: relative;
      height: 600px;
      max-width: 1200px;
      list-style: none;
      // border: 2px solid red;
      display: flex;
      justify-content: center;
      overflow: hidden;
      @media screen and (max-width: $pro){
        height: 500px;
      }
      @media screen and (max-width: $pad){
        height: 400px;
      }
      @media screen and (max-width: $mobile){
        height: 235px;
      }
      >li{
        position: absolute;
        height: 100%;
        >img{
          height: 100%;
          object-fit: cover;
        }
      }
    }
    >ul.inforPoint{
      display: flex;
      list-style: none;
      // border: 1px solid red;
      justify-content: center;
      >li{
        margin: 10px;
        display: flex;
        align-items: center;
        >span{
          display: block;
          width: 8px;
          height: 8px;
          border-radius: 2px;
          transform: rotate(45deg);
          background-color: #be9c6d;
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
  aside{
    >div.inforTop{
      display: flex;
      justify-content: center;
      max-width: 1200px;
      margin: 50px auto;
      // border: 1px solid red;
      >.inforTop_left{
        width: 40%;
        display: flex;
        justify-content: center;
        align-items: center;
        padding-left: 10%;
        @media screen and (max-width: $pad){
          padding-left: 0;
        }
        >img{
          width: 60%;
          @media screen and (max-width: $pad){
            width: 40%;
          }
          @media screen and (max-width: $mobile){
            display: none;
          }
        }
      }
      >.inforTop_right{
        width: 60%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        @media screen and (max-width: $mobile){
          width: 500%;
          }
        >h2{
          font-size: 46px;
          color: #004c34;
          font-weight: 500;
          @media screen and (max-width: $mobile){
            font-size: $pad-title-2;
          }
        }
        >table{
          margin-top: 20px;
          >tbody{
            font-size: 16px;
            vertical-align: top;
            color: #004c34;
            >tr{
              >th{
                text-align: left;
                // border: 1px solid red;
                padding: 0 20px 12px 0;
              }
              >td{
                padding: 0 20px 12px 0;
              }
            }
          }
        }
      }
    }
    >.inforBottom{
      width: 80%;
      max-width: 1200px;
      margin: 0 auto;
      @media screen and (max-width: $pad){
        width: 100%;
      }
      >img{
        width: 90%;
        margin: 0 auto;
        @media screen and (max-width: $mobile){
          width: 90%;
        }
      }
    }
  }



</style>