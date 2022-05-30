
<script>
export default {
  
  }
</script>

<template>
  <a href="#">
    <img src="@/assets/information/logo-yoko.png" alt="">
  </a>
  <div class="text">
    <p>本網頁僅為個人練習用, 絕無做任何商業用途</p>
    <p>如有侵權請來信告知 jay15003939@gmail.com</p>
  </div>
</template>

<style lang="scss" scoped>
  a{
    width: 200px;
    margin: 0 auto 20px;
    // border: 1px solid red;
    display: flex;
    justify-content: center;
    >img{
      width: 100%;
    }
  }
  div.text{
    text-align: center;
    padding: 20px 0;
    font-size: 12px;
    color: #004c34;
    letter-spacing: 2px;
    // border: 1px solid red;
  }
</style>