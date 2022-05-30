
<script>
import Header from '@/components/Header.vue';
import News from '@/components/News.vue';
import History from '@/components/History.vue';
import Menu from '@/components/Menu.vue';
import Information from '@/components/Information.vue';
import Footer from '@/components/Footer.vue';
import { onMounted, reactive } from '@vue/runtime-core';

export default {
  name: 'Home',
  components: {
    Header,
    News,
    History,
    Menu,
    Information,
    Footer
  },
  setup() {
    const yScrollData = reactive({list: []});
    const hisYscrollFn = (hisYscroll) => {
      yScrollData.list.history = hisYscroll;
    }
    const menuYscrollFn = (menuYscroll) => {
      yScrollData.list.menu = menuYscroll;
    }
    const inforYscrollFn = (inforYscroll) => {
      yScrollData.list.infor = inforYscroll;
    }

    const newsYscrollFn = (newsYscroll) => {
      yScrollData.list.news = newsYscroll;
    }

    return{
      hisYscrollFn,
      yScrollData,
      menuYscrollFn,
      inforYscrollFn,
      newsYscrollFn
    }
  }
}
</script>

<template>
  <Header :yScrollData="yScrollData"/>
  <News @newsCallBack="newsYscrollFn"/>
  <History @hisCallBack="hisYscrollFn"/>
  <Menu @menuCallBack="menuYscrollFn" :yScrollData="yScrollData"/>
  <Information @inforCallBack="inforYscrollFn"/>
  <Footer />
</template>

