<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <APlayer></APlayer>
  </div>
</template>

<script>
// @ is an alias to /src
import APlayer from '@/components/aplayer.vue';

export default {
  name: 'home',
  components: {
    APlayer,
  },
};
</script>
