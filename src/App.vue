<template>
  <HeaderBlock />
  <NewsBlock />
  <FooterBlock />
</template>

<script>
import HeaderBlock from "./components/HeaderBlock.vue";
import NewsBlock from "./components/NewsBlock.vue";
import FooterBlock from "./components/FooterBlock.vue";

export default {
  name: "App",
  components: {
    HeaderBlock,
    NewsBlock,
    FooterBlock,
  },
};
</script>

<style>
body {
  margin: 0 auto;
  max-width: 1920px;
  width: 100%;
}
.container {
  padding: 0 100px;
}
@media screen and (max-width: 1024px) {
  .container {
    padding: 0 40px;
  }
}

@media screen and (max-width: 768px) {
  .container {
    padding: 0 12px;
  }
}
#app {
  font-family: "Nunito Sans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  box-sizing: border-box;
}
</style>
