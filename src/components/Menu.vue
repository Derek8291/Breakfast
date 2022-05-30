
<script>
import { menuList } from "@/lib/data.js";
import { reactive, ref } from '@vue/reactivity';
import { onMounted, onUnmounted, onUpdated, watch } from '@vue/runtime-core';
import { useRouter } from "vue-router";
export default {
  emits: ["menuCallBack"],
  props: ["yScrollData"],
  setup(props, { emit }){
    const router = useRouter();
    const menuArr = reactive(menuList);
    const menuOpen = ref(false);
    const shopOpen = ref(false);
    const emptyFn = ref(true);
    const cartArr = reactive({list: []});
    const cartArrCount = ref(0);
    const cartArrRecord = reactive({list: []});
    const goTop = ref(false);
    const Yscroll = ref(null);
    const closeMenuBtn = ref(null);
    const handMenuOpen = () => {
      if(menuOpen.value){
        menuOpen.value = false;
        closeMenuBtn.value.innerText = "SHOW MENU"
      }else{
        menuOpen.value = true;
        closeMenuBtn.value.innerText = "ClOSE MENU"
        // console.log(closeMenuBtn.value);
        let top = document.documentElement.scrollTop || document.body.scrollTop;
        const timeTop = setInterval(() => {
          if(props.yScrollData.list.menu > top){
            document.body.scrollTop = document.documentElement.scrollTop = top += 50
            if (top > props.scrollMove[0].list.history + 100) {
              clearInterval(timeTop)
            }
          }else{
            document.body.scrollTop = document.documentElement.scrollTop = top -= 50
            if (top <= props.yScrollData.list.menu) {
              clearInterval(timeTop)
            }
          }
        }, 10)
      }
    };
    const handCartClose = () => {
      shopOpen.value = !shopOpen.value;
    }
    const addCart = (idx, e) => {
      const nameCut = e.target.innerText.indexOf('・');
      const productName = e.target.innerText.substr(0, nameCut);
      const productDetail = menuArr[idx][productName];
      if(cartArrRecord.list.includes(productDetail.name)){
        productDetail.count++;
        cartArrCount.value++;
      }else{
        cartArr.list.push(productDetail);
        cartArrRecord.list.push(productDetail.name);
        cartArrCount.value++;
        console.log(cartArr);
      }
    };
    const handPlus = (item) => {
      item.count++;
      cartArrCount.value++;
    }
    const handReduce = (item) => {
      if(item.count < 2) return;
      item.count--;
      cartArrCount.value--;
    }
    const handDelete = (item) => {
      cartArrCount.value = cartArrCount.value - item.count;
      cartArr.list =  cartArr.list.filter((e) => {
        return e !== item;
      })
      cartArrRecord.list = cartArrRecord.list.filter((e) => {
        return e != item.name;
      })
      item.count = 1;

    }
 
    let newArr = null;

    const gotoBuy = () => {
      newArr = JSON.stringify(cartArr);
      router.push({
        path: '/checkout',
        query: {
          id: newArr,
          name: 'newArr',
        }
      });
      shopOpen.value = !shopOpen.value;

      };

    onMounted(() => {
      watch(cartArrRecord, (e) => {
        if(e.list.length > 0){
          emptyFn.value = false;
        }else{
          emptyFn.value = true;
        }
      });

      window.addEventListener("scroll", order);

      function order(){
        if(window.scrollY > 300){
          goTop.value = true;
        }else{
          goTop.value = false;
        };
      };
      
      let menuYscroll = Yscroll._value.offsetTop;
      emit("menuCallBack", menuYscroll);

    });

    const goTopFn = () => {
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      const timeTop = setInterval(() => {
        document.body.scrollTop = document.documentElement.scrollTop = top -= 100
        if (top <= 0) {
          clearInterval(timeTop)
        }
      }, 10)
    };


    return {
      menuArr,
      handMenuOpen,
      menuOpen,
      addCart,
      cartArr,
      handPlus,
      handReduce,
      handDelete,
      handCartClose,
      shopOpen,
      gotoBuy,
      emptyFn,
      cartArrCount,
      goTopFn,
      goTop,
      Yscroll,
      closeMenuBtn
    };
  }
  }
</script>

<template>
  <article>
    <ul class="icon">
      <li>
        <a href="javascript:;" @click="handCartClose">
          <i class="fas fa-shopping-cart"></i>
          <span v-if="cartArrCount">{{cartArrCount}}</span>
        </a>
      </li>
      <li>
        <a href="https://www.instagram.com/wm.taiwanbreakfast/" target="_blank">
          <i class="fab fa-instagram"></i>
        </a>
      </li>
    </ul>
    <span @click="goTopFn" v-show="goTop" :class="goTop ? 'goTopAfter' : ''">Back to Top</span>
    <div class="title" id="title_position">
      <h1 ref="Yscroll">Menu</h1>
      <p>搾りたての自家製豆乳に、豆乳スープの鹹豆漿。具材にこだわった台湾おにぎりの飯糰など、
  台湾朝食の新習慣を皆様にお届けいたします。</p>
    </div>
    <ul class="content" >
      <li class="item" v-for="(item, idx) in menuArr" :key="item.id">
        <div class="itemImg">
          <img :src="item.imgUrl" alt="">
          <ul class="detail" :class="{detail_show: menuOpen}">
            <li v-if="item.list1">
              <span></span>
              <p @click="addCart(idx, $event)" :id="item.money1">{{item.list1}}</p>
            </li>
            <li v-if="item.list2">
              <span></span>
              <p @click="addCart(idx, $event)" :id="item.money2">{{item.list2}}</p>
            </li>
            <li v-if="item.list3">
              <span></span>
              <p @click="addCart(idx, $event)" :id="item.money3">{{item.list3}}</p>
            </li>
            <li v-if="item.list4">
              <span></span>
              <p @click="addCart(idx, $event)" :id="item.money4">{{item.list4}}</p>
            </li>
          </ul>
        </div>
        <div class="itemTxt">
          <p>{{item.content}}</p>
          <img :src="item.nameUrl" alt="">
        </div>
      </li>
    </ul>
    <div class="menuBtn">
      <a href="javascript:;" @click="handMenuOpen" ref="closeMenuBtn">SHOW MENU</a>
    </div>
    <h3 v-for="item in cartArr" :key="item.id">{{item.name}}</h3>
  </article>
  <aside class="shopCart" v-show="shopOpen">
    <button @click="handCartClose">

    </button>
    <div class="shopCart_title">
      <p>商品</p>
      <p>單價</p>
      <p>數量</p>
      <p>總計</p>
      <p>操作</p>
    </div>
    <ul>
      <p v-show="emptyFn">無商品</p>
      <li v-for="item in cartArr.list" :key="item.name">
        <ul>
          <li>
            <img :src="item.imgUrl" alt="">
          </li>
          <li>
            <p>{{item.name}}</p>
          </li>
          <li>
            <p>{{item.price}}</p>
          </li>
          <li>
            <span @click="handReduce(item)">-</span>
            <p>{{item.count}}</p>
            <span @click="handPlus(item)">+</span>
          </li>
          <li>
            <p>{{item.price * item.count}}</p>
          </li> 
          <li>
            <button @click="handDelete(item)">刪除</button>
          </li>
        </ul>
      </li>
    </ul>
    <div class="cartSubmit">
      <button href="" @click="gotoBuy">去買單</button>
    </div>
  </aside>
</template>

<style lang="scss" scoped>
  article{
    $pro: 1025px;
    $pad: 769px;
    $mobile: 376px;
    $pad-title-1: 36px;
    $pad-title-2: 26px;
    width: 100%;
    > ul.icon{
        position: fixed;
        top: 20px;
        left: 20px;
        list-style: none;
        display: flex;
        background-color: transparent;
        z-index: 1;
        >li{
          margin-right: 15px;
          background-color: transparent;
          :hover{
            animation: iconShake .5s infinite alternate linear;
          }
          >a{
            background-color: transparent;
            position: relative;
            >i{
              background-color: transparent;
              color: #004c34;
              font-size: 20px;
            }
            >span{
              position: absolute;
              top: -8px;
              left: -8px;
              background-color: red;
              color: #fff;
              font-size: 14px;
              padding: 0 5px;
              border-radius: 50%;
            }
          }
        }
      }
      >span{
        position: fixed;
        bottom: 50px;
        right: 50px;
        height: 20px;
        padding-top: 30px;
        font-size: 16px;
        color: #be9c6d;
        z-index: 1;
        cursor: pointer;
        @media screen and (max-width: $pro){
          bottom: 40px;
          right: 20px;
        }
        &::before {
          content: "";
          position: absolute;
          top: 10%;
          left: 37%;
          width: 5px;
          height: 20px;
          border-radius: 3px;
          background-color: #be9c6d;
          transform: rotate(30deg);
        }
        &::after {
          content: "";
          position: absolute;
          top: 10%;
          right: 37%;
          width: 5px;
          height: 20px;
          border-radius: 3px;
          background-color: #be9c6d;
          transform: rotate(-30deg);
        }
      }
    >div.title{
      text-align: center;
      color: #004c34;
      margin: 50px 0;
      >h1{
        font-size: 56px;
        font-weight: 400;
        @media screen and (max-width: 768px){
          font-size: $pad-title-1;
        }
      } 
      >p{
        font-size: 16px;
        max-width: 600px;
        width: 80%;
        // border: 1px solid red;
        margin: 20px auto 0;
        line-height: 1.8;
        @media screen and (max-width: 376px){
          width: 100%;
          padding: 0 5px;
        }
      }
    }
    >ul.content{
      max-width: 1200px;
      width: 100%;
      margin: 80px auto;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      @media screen and (max-width: $pad){
        margin: 150px auto;
      }
      >li.item{
        display: flex;
        width: 50%;
        height: 220px;
        justify-content: space-between;
        margin-bottom: 100px;
        &:hover {
          >div.itemImg{
            >ul.detail{
              opacity: 1;
            }
          }
          }
        &:nth-of-type(4n-1){
          flex-direction: row-reverse;
          @media screen and (max-width: $pro){
            flex-direction: row;
          }
        }
        &:nth-of-type(4n){
          flex-direction: row-reverse;
          @media screen and (max-width: $pro){
            flex-direction: row;
          }
        }
        @media screen and (max-width: $pro) {
          width: 100%;
          align-items: center;
          justify-content: center;
          &:nth-of-type(even){
            flex-direction: row-reverse;
          }
        }
        @media screen and (max-width: $pad) {
          &:nth-of-type(even){
            flex-direction:column;
            margin-bottom: 270px;
          }
          &:nth-of-type(odd){
            flex-direction:column;
            margin-bottom: 270px;
          }
        }

        >div.itemImg{
          height: auto;
          width: 50%;
          position: relative;
          // border: 1px solid red;
          @media screen and (max-width: $pro){
            padding: 0 2%;
          }
          @media screen and (max-width: $pad){
            margin-bottom: 10px;
            width: auto;
            max-width: 350px;
          }
          >img{
            height: 220px;
            width: 100%;
            object-fit: cover;
          }
          >ul.detail{
            position: absolute;
            top: 0;
            right: 0;
            padding: 10px;
            width: 100%;
            height: 220px;
            background-color: rgba(0,0,0,.4);
            list-style: none;
            backdrop-filter: blur(5px);
            display: flex;
            justify-content: center;
            opacity: 0;
            transition: .3s;
             @media screen and (max-width: $pad){
               margin-right: 3%;
               width: 94%;
             }
            &.detail_show{
              opacity: 1;
            }
          >li{
            writing-mode: vertical-rl;
            // border: 1px solid red;
            float: right;
            padding: 5px 10px;
            display: flex;
            align-items: center;
            color: #fff;
            font-weight: 400;
            cursor: pointer;
            &:hover{
              color: #f6dbc7;
            }
            &:hover span{
              border: 2px solid #f6dbc7;
            }
            &:hover span::before{
            background-color: #f6dbc7;
            }
            &:hover span::after{
            background-color: #f6dbc7;
            }
            >span{
              width: 22px;
              height: 22px;
              border: 2px solid #fff;
              border-radius: 50%;
              margin: 0 0 8px;
              position: relative;
              &::before{
                content: "";
                position: absolute;
                bottom: calc(50% - 1px);
                left: 20%;
                width: 60%;
                height: 2px;
                background-color: #fff;
              }
              &::after{
                content: "";
                position: absolute;
                left: calc(50% - 1px);
                bottom: 20%;
                height: 60%;
                width: 2px;
                background-color: #fff;
              }
            }
            >p{
              position: relative;
              &:before{
                content: "";
                position: absolute;
                left: 3px;
                top: -29px;
                width: 20px;
                height: 20px;
                background-color: transparent;
              }
            }
          }
        }
        }
        >div.itemTxt{
          display: flex;
          align-items: flex-start;
          justify-content: space-around;
          height: 100%;
          width: 50%;
          padding: 0 5px;
          // border: 1px solid red;
          @media screen and (max-width: $pro){
            width: 30%;
          }
          @media screen and (max-width: $pad){
            width: 50%;
          }
          >p{
            writing-mode: vertical-rl;
            font-size: 16px;
            line-height: 1.8;
            color: #004c34;
          }
          >img{
            width: 20%;
            @media screen and (max-width: $pro){
              width: 15%;
            }
            @media screen and (max-width: $pad){
              width: 50px;
              margin-left: 10px;
            }
          }
        }
        &:last-of-type{
          width: 80%;
          margin: 0 auto;
          @media screen and (max-width: $pro){
              width: 100%;
          }
          >div.itemTxt{
            >img{
              width: 28%;
            }
          }
        }
      }
    }
    >div.menuBtn{
      // border: 1px solid red;
      display: flex;
      justify-content: center;
      >a{
        color: #be9c6d;
        text-decoration: none;
        border: 4px solid #be9c6d;
        border-radius: 10px;
        padding: 30px 100px;
        transition: all 0.3s;
        @media screen and (max-width: $pad){
          border-radius: 5px;
          padding: 15px 50px;
        }
        &:hover{
          background-color: #be9c6d;
          color: #fff;
        }
      }
    }
  }
  aside.shopCart{
    position: fixed;
    top: 0;
    left: 0;
    overflow: auto;
    width: 50%;
    height: 100%; 
    max-width: 800px;
    background-color: #fff;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, .5);
    color: #be9c6d;
    z-index: 999;
    @media screen and (max-width: 1025px){
      width: 60%;
    } 
    @media screen and (max-width: 769px){
      width: 100%;
    } 
    >button{
      position: relative;
      top: 12px;
      left: 24px;
      border: none;
      width: 20px;
      height: 20px;
      // border: 1px solid red;
      background-color: transparent;
      transition: transform .2s;
      cursor: pointer;
      &:hover{
      transform: scale(1.3);
      }
      &::before{
        content: "";
        position: absolute;
        left: -1px;
        bottom: 8px;
        width: 20px;
        height: 2px;
        background-color: #be9c6d;
        transform: rotate(45deg);
      }
      &::after{
        content: "";
        position: absolute;
        left: 8px;
        bottom: -1px;
        width: 2px;
        height: 20px;
        background-color: #be9c6d;
        transform: rotate(45deg);
      }
    }
    >div.shopCart_title{
      width: 100%;
      // margin: 10px auto;
      display: flex;
      padding: 10px 0;
      border-bottom: 3px double #be9c6d;
      font-weight: 400;
      font-size: 16px;
      p{
        text-align: center;
        // border: 1px solid red;
        &:nth-of-type(1){
          width: 50%;
        }
        &:nth-of-type(2){
          width: 10%;
        }
        &:nth-of-type(3){
          width: 15%;
        }
        &:nth-of-type(4){
          width: 15%;
        }
        &:nth-of-type(5){
          width: 10%;
        }
        
      }
    }
    >ul{
      display: flex;
      flex-direction: column;
      list-style: none;
      margin: 10px auto;
      color: #004c34;
      // border: 1px solid red;
      >p{
        text-align: center;
      }
      >li{
        >ul{
          list-style: none;
          display: flex;
          align-self: center;
          padding: 0 1%;
          >li{
            display: flex;
            align-self: center;
            justify-content: center;
            font-size: 16px;
            &:nth-of-type(1){
              width: 20%;
              // border: 1px solid blue;
              img{
                width: 100px;
                height: 80px;
                padding: 5px 0;
                border-radius: 10px;
                object-fit: cover;
              }
            }
            &:nth-of-type(2){
              width: 30%;
            }
            &:nth-of-type(3){
              width: 10%;
            }
            &:nth-of-type(4){
              width: 15%;
              
              >p{
                margin: 0 10px;
              }
              >span{
                border: 1px solid rgba(0, 76, 52, .5);
                box-shadow: 1px 1px 1px rgba(0, 76, 52, .5);
                padding: 0 3px;
                cursor: pointer;
                transition: all 0.2s;
                @media screen and (max-width: 376px){
                  padding: 0 1px;
                }
                &:hover{
                  background-color: rgba(0, 76, 52, .8);
                  color: #fff;
                }
              }
            }
            &:nth-of-type(5){
              width: 15%;
            }
            &:nth-of-type(6){
              width: 10%;
              >button{
                font-size: 14px;
                padding: 2px 5px;
                background-color: transparent;
                border: 1px solid #004c34;
                border-radius: 3px;
                color: #004c34;
                cursor: pointer;
                transition: all .3s;
                &:hover{
                  background-color: rgba(0, 76, 52, .8);
                  color: #fff;
                }
              }
            }
          }
        }
      }
    }
    >div.cartSubmit{
      display: flex;
      justify-content: center;
      border-top: 3px double #be9c6d;
      padding: 15px;
      >button{
        font-size: 16px;
        padding: 5px 15px;
        background-color: transparent;
        border:2px solid #be9c6d;
        border-radius: 3px;
        cursor: pointer;
        color: #be9c6d;
        transition: all .3s;
        &:hover{
          background-color: #be9c6d;
          color: #fff;
        }
        }
    }
  }

  @keyframes iconShake{
    // 0%{
    //   transform: rotate(0deg);
    // }
    // 33%{
    //   transform: rotate(10deg);
    // }
    // 66%{
    //   transform: rotate(0deg);
    // }
    // 100%{
    //   transform: rotate(10deg);
    // }
    0%{
      transform: rotate(0deg);
    }
    50%{
      transform: rotate(10deg);
    }
    75%{
      transform: rotate(0deg);
    }
    100%{
      transform: rotate(-10deg);
    }
  }
</style>