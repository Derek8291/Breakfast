
<script>
import { topicList } from "@/lib/data.js";
import { reactive, ref} from '@vue/reactivity';
import { onMounted } from '@vue/runtime-core';
export default {
  emits: ["newsCallBack"],
  setup(props, { emit }){
    const listArr = reactive(topicList);
    const Yscroll = ref(null);


    onMounted(() => {
      let newsYscroll = Yscroll._value.offsetTop;
      emit("newsCallBack", newsYscroll);
    })


    return{
      listArr,
      Yscroll
    }
  }
  }
</script>

<template>
  <article>
    <div class="title" ref="Yscroll">
      <h1>NEWS</h1>
    </div>
    <ul>
      <li class="topic_item" v-for="(item, idx) in listArr" :key="item.date" >
        <a href="">
          <div class="topic_item_top" v-if="idx < 3">
            <p>NEW POST!</p>
            <h1>{{item.date}}</h1>
            <p>{{item.time}}</p>
          </div>
          <div class="topic_item_bottom" v-if="idx < 3">
            <img :src="item.imgUrl" alt="">
            <p>{{item.title}}</p>
          </div>
        </a>
      </li>
    </ul>
  </article>
  <div class="clearFloat"></div>
</template>

<style lang="scss" scoped>
  $pad: 769px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;

  article{
    width: 100%;
    height: 550px;
    margin-top: 70px;
    background: url(../assets/topic/bg-moyo01.png);
    background-position: 0% 0%;
    background-repeat:no-repeat;
    background-size: 35% auto;
    // border: 1px solid red;
    >div.title{
    text-align: center;
    color: #004c34;
      >h1{
        font-size: 56px;
        font-weight: 400;
        @media screen and (max-width: $pad){
          font-size: $pad-title-1;
        }
      } 
    }
    >ul{
      margin: auto;
      width: 100%;
      max-width: 1000px;
      padding: 30px 0;
      >li.topic_item{
        // border: 1px solid red;
        float: left;
        display: flex;
        width: 32%;
        flex-direction: column;
        align-items: center;
        & + .topic_item{
          margin-left: 2%;
        }
        >a{
          text-decoration: none;
          color: #004c34;
          display: block;
          >.topic_item_top{
            text-align: center;
            padding-bottom: 30px;
            >h1{
              font-size: $pad-title-2;
              font-weight: 500;
              letter-spacing: 3px;
              margin: 2px;
            }
            >p{
              font-size: 14px;
              letter-spacing: 1px;
              background-color: transparent;
            }
          }
          >.topic_item_bottom{
            box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
            position: relative;
            >img{
              width: 100%;
              height: auto;
            }
            >p{
              background-color: #fff;
              font-size: 14px;
              padding: 25px 15px;
              height: auto;
            }
            &::after{
              content: "";
              position: absolute;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
              transition: background-color 0.3s;
              background-color: transparent;
            }
          }
          &:hover .topic_item_bottom::after{
            background-color: rgba(251, 248, 246, 0.5);
          }
        }
      }
    }
  }
  .clearFloat{
    clear: both;
  }
</style>