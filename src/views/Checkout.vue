
<script>
import { onMounted } from '@vue/runtime-core';
import { useRouter } from "vue-router";
import Shopcart from '@/components/Shopcart.vue';

export default {
  components:{
    Shopcart,

  },

  setup() {
    const router = useRouter();
    onMounted(() => {

    })

    return{
      router
    }
  }
  }
</script>
 
<template>
  <Shopcart :msg="JSON.parse(this.$route.query.id)"/>
  <td></td>
</template>

<style>

</style>
