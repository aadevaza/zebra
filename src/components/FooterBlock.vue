<template>
  <section class="footer">
    <div class="footer__logo-container container">
      <div class="footer__logo">
        <a href="#">
          <img class="footer__logo-img" src="~assets/logo.png" alt="" />
        </a>
      </div>
      <p class="footer__copyright">© alpha, {{ currentDate }}</p>
    </div>
  </section>
</template>

<script>
export default {
  name: "FooterBlock",
  setup() {
    const currentDate = new Date().getFullYear();

    return {
      currentDate,
    };
  },
};
</script>

<style scoped lang="scss">
.footer {
  background-color: #F0F6FE;

  &__logo-container {
    padding-top: 64px;
    padding-bottom: 64px;
    max-width: 114px;
    width: 100%;
  }

  &__logo {
    img {
      width: 100%;
    }
  }

  &__copyright {
    margin: 0;
    margin-top: 16px;
  }
}
</style>
