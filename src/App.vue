<template>
  <div id="background">
    <div id="app">
      <PostList />
    </div>
  </div>
</template>

<script>
import PostList from './components/PostList.vue';

export default {
  name: 'App',
  components: {
    PostList
  }
};
</script>

<style>
#background {
  height: 100vh;
  width: 100vw;
  background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQMXwXcwzjv6l6kvmR1RLZQFMW2w3fSHxlJ4Q&s'); /* Ganti dengan link gambar Anda */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
