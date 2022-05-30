
<script>
import { computed, onBeforeMount, onMounted, ref, reactive} from '@vue/runtime-core';
export default {
  props: ["msg"],
  // data() {
  //   return{
  //     sum: 0,
  //   }
  // },

  setup(props) {

    let newArr = ref('');
    const cartIsOpen = ref(true);
    const sum = ref(0);
    const isOpen = ref(true);
    const dateGet = reactive({buyerDate: '', buyerName: '', buyerPhone: ''});
    const submitOpen = ref(false);
    const dateCheck = ref(false);
    const nameCheck = ref(false);
    const phoneCheck = ref(false);
    const typePhone = ref('');
    let phoneNum = ref('請輸入手機號碼');

    newArr = props.msg.list;

    onBeforeMount(() => {

      function cartConfirm(){
        if(newArr.length > 0){
          cartIsOpen.value = false;
        }else{
          cartIsOpen.value = true;
        }
      }
      cartConfirm();

      function SumDataforEach(arr){
        arr.forEach((e) => {
          sum.value += Number(e.price * e.count);
        });
      }

      SumDataforEach(newArr);

    })


    const payMethod = () => {
      isOpen.value = !isOpen.value;
    }

    const checkOut = () => {
      let openIdx = 0;
      let reg = /[0-9]/g;
      dateCheck.value = false;
      nameCheck.value = false;
      phoneCheck.value = false;
      if((dateGet.buyerDate).trim() === ''){
        dateCheck.value = true;
      }else{
        openIdx++;
      };

      if((dateGet.buyerName).trim() === ''){
        nameCheck.value = true;
      }else{
        openIdx++;
      };

      if((dateGet.buyerPhone).trim() === ''){
        phoneCheck.value = true;
      }else if(Number((dateGet.buyerPhone).trim()[0]) !== 0 || Number((dateGet.buyerPhone).trim()[1]) !== 9){
        console.log((dateGet.buyerPhone).trim()[0]);
        phoneNum.value = "請輸入正確手機號碼";
        phoneCheck.value = true;
      }else if((dateGet.buyerPhone).trim().length !== 10){
        phoneNum.value = "請輸入十位手機號碼";
        phoneCheck.value = true;
      }else if(!reg.test((dateGet.buyerPhone).trim())){
        phoneNum.value = "請輸入數字";
        phoneCheck.value = true;
      }else{
        openIdx++;
      }

      if(openIdx === 3){
        submitOpen.value = true;
      }
    };

    const submitClose = () => {
      submitOpen.value = false;
    }



    return {
      props,
      newArr,
      cartIsOpen,
      sum,
      payMethod,
      isOpen,
      dateGet,
      checkOut,
      submitOpen,
      dateCheck,
      nameCheck,
      phoneCheck,
      phoneNum,
      submitClose
    }
  }
  }
</script>

<template>
  <router-link href="" class="logo" to="/">
    <img src="@/assets/header/logo-tate.png" alt="">
  </router-link>
  <article class="shopCart">
    <!-- <button>
    </button> -->
    <div class="shopCart_title">
      <p>訂購商品</p>
      <p>單價</p>
      <p>數量</p>
      <p>總計</p>
    </div>
    <ul>
      <p v-if="cartIsOpen">無商品</p>
      <li v-for="item in newArr" :key="item.name">
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
            <!-- <span>-</span> -->
            <p>{{item.count}}</p>
            <!-- <span>+</span> -->
          </li>
          <li>
            <p>{{item.price * item.count}}円</p>
          </li> 
          <!-- <li>
            <button>刪除</button>
          </li> -->
        </ul>
      </li>
    </ul>
  </article>
  <article class="payMent" v-if="!cartIsOpen">
    <div class="payMent_title">
      <p>付款方式 :</p>
      <ul>
        <li>
          <a href="javascript:;" @click="payMethod" :class="[{backChange: isOpen}]">現場付款</a>
        </li>
        <li>
          <a href="javascript:;" @click="payMethod" :class="[{backChange: !isOpen}]">銀行轉帳</a>
        </li>
      </ul>
    </div>
    <div class="payMent_content">
        <form action="">
          <div>
            <label for="chooseDate">請選擇日期 :</label>
            <input type="date" id="chooseDate" min="2022-01-15" max="2022-01-31" v-model="dateGet.buyerDate">
            <p v-show="dateCheck">請輸入日期</p>
          </div>
          <div class="bankPay_name">
            <label for="typeName">訂購人姓名 :</label>
            <input type="text" id="typeName" v-model="dateGet.buyerName">
            <p v-show="nameCheck">請輸入姓名</p>
          </div>
          <div class="bankPay_phone">
            <label for="typePhone">訂購人電話 :</label>
            <input type="text" id="typePhone" v-model="dateGet.buyerPhone">
            <p v-show="phoneCheck" >{{phoneNum}}</p>
          </div>
        </form>
        <div v-show="!isOpen">
          <span>銀行帳戶 : </span>
          <span>11501234567890 (807永豐)</span>
        </div>
    </div>
    <div class="payMent_submit">
      <p>商品總金額 : {{sum}} 円</p>
      <button @click="checkOut">下訂單</button>
    </div>
  </article>
  <aside v-if="submitOpen">
    <div class="successContent">
      <h1>訂購成功</h1>
      <p>取餐時間 : {{dateGet.buyerDate}}</p>
      <p>(早上10點 ~ 中午12點)</p>
      <p>訂購人姓名 : {{dateGet.buyerName}}</p>
      <p>訂購人電話 : {{dateGet.buyerPhone}}</p>
      <button @click="submitClose"></button>
    </div>
  </aside>

</template>

<style lang="scss" scoped>
  $pro: 1025px;
  $pad: 769px;
  $pad-title-0: 46px;
  $pad-title-1: 36px;
  $pad-title-2: 26px;
  $mobile: 376px;
  $mobile-title-2: 20px;
  .logo{
    width: 100%;
    >img{
      width: 100px;
      margin: 50px auto;
    }
  }
  article.shopCart{
    overflow: auto;
    width: 60%;
    margin: 50px auto;
    height: auto; 
    max-width: 800px;
    background-color: #fff;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, .5);
    color: #be9c6d;
    @media screen and (max-width: $pro){
      width: 80%;
    }
    @media screen and (max-width: $pad){
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
        width: 25%;
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
        padding-left: 5%;
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
              &:hover{
                background-color: rgba(0, 76, 52, .8);
                color: #fff;
              }
            }
          }
          &:nth-of-type(5){
            width: 25%;
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
article.payMent{
    overflow: auto;
  width: 60%;
  margin: 50px auto;
  height: auto; 
  max-width: 800px;
  background-color: #fff;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, .5);
  color: #be9c6d;
  @media screen and (max-width: $pro){
    width: 80%;
  }
  @media screen and (max-width: $pad){
    width: 100%;
  }
  >div.payMent_title{
    display: flex;
    padding: 25px 0;
    border-bottom: 3px double #be9c6d;
    font-weight: 400;
    font-size: 16px;
    justify-content: center;
    >p{
      text-align: center;
      margin: 0 20px;
    }
    >ul{
      display: flex;
      list-style-type: none;

      >li{
        margin-right: 20px;
        >a{
          text-decoration: none;
          color: #004c34;
          border: 1px solid rgba(0, 76, 52, .9);
          padding: 3px 10px;
          transition: .3s;
          &.backChange{
            background-color: rgba(0, 76, 52, .9);
            color: #fff;
          }
        }
      }
    }
  }
  >div.payMent_content{
    height: 230px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: #004c34;
    font-size: 16px;
      >form{
        >div{
          margin: 15px;
          >label{
            margin: 15px;
          }
          >input{
            width: 180px;
            outline: none;
          }
          >p{
            color: red;
            margin-left: 120px;
          }
        }
      }
      >div{
        margin-left: 22px;
        >span{
          & + span{
            margin-left: 10px;
          }
        }
      }
  }
  >div.payMent_submit{
    display: flex;
    justify-content: flex-end;
    align-items: center;
    border-top: 3px double #be9c6d;
    font-weight: 400;
    font-size: 16px;
    padding: 10px 0;
    >p{

    }
    >button{
      margin: 0 30px;
      background-color: rgba(0, 76, 52, .9);
      border: 1px solid rgba(0, 76, 52, .9);
      color: #fff;
      font-size: 26px;
      padding: 5px 10px;
      cursor: pointer;
    }
  }
}
aside{
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, .4);
  backdrop-filter: blur(3px);
  display: flex;
  justify-content: center;
  align-items: center;
  >div.successContent{
    border:1px solid#004c34;
    background-color: #fff;
    padding: 20px 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 400px;
    border-radius: 10px;
    position: relative;
    color: #004c34;
    >h1{
      font-weight: 500;
      margin: 20px;
      padding-bottom: 3px;
      border-bottom: 3px double #004c34;
      width: 100%;
      text-align: center;
    }
    >p{
      margin-bottom: 20px;
    }
    >button{
      position: absolute;
      top: 15px;
      right: 15px;
      width: 30px;
      height: 30px;
      background-color: transparent;
      border: 1px solid transparent;
      transform: rotate(45deg);
      cursor: pointer;
      &::before{
        content: "";
        position: absolute;
        width: 100%;
        height: 2px;
        background-color: #004c34;
        left: 0;
        top: 12px;
      }
      &::after{
        content: "";
        position: absolute;
        height: 100%;
        width: 2px;
        background-color: #004c34;
        left: 12px;
        top: 0;
      }
    }
  }
}
</style>