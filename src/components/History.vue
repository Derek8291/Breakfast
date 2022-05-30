
<script>
import { ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core';

export default {
  emits: ["hisCallBack"],
  setup(props, { emit }){
    const mainVideo = ref(null);
    const soundOpen = ref(true);
    const Yscroll = ref(null);
    const soundHandle = () => {
      soundOpen.value = !soundOpen.value;
    }
      onMounted(() => {
        let hisYscroll = Yscroll._value.offsetTop;
        emit("hisCallBack", hisYscroll)
        })
    return{
      mainVideo,
      soundOpen,
      soundHandle,
      Yscroll
    }
  }
  }

</script>

<template>
  <article>
    <div class="history_top" ref="Yscroll">
      <img src="@/assets/history/conceptttl.png" alt="">
      <img src="@/assets/history/conceptttlsub.png" alt="">
      <img src="@/assets/history/conceptcatch.png" alt="">
      <div class="history_txt">
        <p>“早餐（ザオツァン）”中国語で朝食を意味するこの言葉。</p>
        <p>外食文化が発達している台湾には、早餐店の看板を掲げた食堂や屋台が多くあります。</p>
        <p>台湾の人々にとって欠かせない、心が躍り出すような多彩な台湾の朝食。</p>
        <p>ワナマナでは、そんなエネルギーを秘めた台湾の朝食と共に</p>
        <p>希望溢れる一日の始まりをお届けします。</p>
      </div>
      <h2>Always HOPEs, always breakfast.</h2>
    </div>
    <div class="history_bottom">
      <video ref="mainVideo" autoplay loop :muted="soundOpen">
        <source src="../assets/history/IMG_0481.mp4" type="video/mp4">
      </video>
      <div class="soundBtn" >
        <div :class="['soundOn', {toOpacity:soundOpen }]" @click="soundHandle">
          <i class="fas fa-volume-up"></i>
          <p>ON</p>
        </div>
        <div :class="['soundOff', {toOpacity: !soundOpen}]" @click="soundHandle">
          <i class="fas fa-volume-mute"></i>
          <p>OFF</p>
        </div>
      </div>
    </div>
  </article>
</template>

<style lang="scss" scoped>
  $pro: 1025px;
  $pad: 769px;
  $pad-title-0: 46px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;
  $mobile: 376px;
  $mobile-title-2: 18px;

  article{
    // border: 1px solid red;
    display: flex;
    justify-content: center;
    flex-direction: column;
    margin-top: 100px;
    @media screen and (max-width: $pad) {
      margin-top: 0;
    }
    @media screen and (max-width: $mobile) {
      margin-top: -80px;
    }
    >div.history_top{
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #004c34;
      >img{
        margin-bottom: 30px;
      }
      >img:nth-child(1){
        max-width: 380px;
        @media screen and (max-width: $pad){
          max-width: 280px;
        }
      }
      >img:nth-child(2){
        width: 380px;
        @media screen and (max-width: $pad){
          max-width: 280px;
        }
      }
      >img:nth-child(3){
        width: 690px;
        @media screen and (max-width: $pad){
          max-width: 380px;
        }
          @media screen and (max-width: $mobile){
            max-width: 300px;
          }

      }
      >div.history_txt{
        margin-bottom: 30px;
        font-size: 16px;
        line-height: 1.8;
        text-align: center;
        letter-spacing: 1px;
        width: auto;
        @media screen and (max-width: $mobile){
          line-height: 1.6;
          letter-spacing: 0px;
          padding: 0 5px;
        }
      }
      >h2{
        font-weight: 400;
        @media screen and (max-width: $pad){
          font-size: $pad-title-2;
        }
        @media screen and (max-width: $mobile){
          font-size: $mobile-title-2;
        }
      }
    }
    >div.history_bottom{
        width: 100%;
        margin: 50px auto;
        vertical-align: middle;
        font-size: 0;
        display: flex;
        justify-content: center;
        // border: 1px solid red;
        position: relative;
      >video{
        max-width: 1200px;
        width: 80%;
        height: auto;
        @media screen and (max-width: $pad){
          width: 100%;
        }
      }
      >div.soundBtn{
        font-size: 20px;
        color: #fff;
        font-weight: 300;
        position: absolute;
        width: 80%;
        max-width: 1200px;
        bottom: 3%;
        padding-right: 20px;
        display: flex;
        justify-content: flex-end;
        // border: 1px solid red;
        @media screen and (max-width: $pad){
          width: 100%;
        }
        >div{
          display: flex;
          align-items: center;
          margin: 0 5px;
          cursor: pointer;
          >p{
            margin: 0 8px;
          }
          &.toOpacity{
            opacity: .5;
          }
        }

      }
    }
    
  }
</style>