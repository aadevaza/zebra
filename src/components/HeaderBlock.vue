<template>
  <section class="header">
    <div class="header__logo-container container">
      <div class="header__logo">
        <a href="#">
          <img class="header__logo-img" src="~assets/logo.png" alt="" />
        </a>
      </div>
    </div>
    <div class="header__teaser">
      <h1 class="header__teaser-title">Новости</h1>
    </div>
  </section>
</template>

<script>
export default {
  name: "HeaderBlock",
};
</script>

<style scoped lang="scss">
.header {
  &__logo-container {
    padding-top: 35px;
    padding-bottom: 35px;
    max-width: 156px;
    width: 100%;
  }

  &__logo {
    img {
      width: 100%;
    }
  }

  &__teaser {
    width: 100%;
    height: 320px;
    background: url("~assets/teaser_bg.jpg") no-repeat;
    background-size: cover;
    background-position: center;
    padding-bottom: 48px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
  }

  &__teaser-title {
    margin-top: auto;
    padding: 0 100px;
    font-size: 40px;
    line-height: 48px;
    font-weight: 700;
    letter-spacing: -1%;
    color: #17171a;
  }

  @media screen and (max-width: 1024px) {
    &__teaser {
      height: 240px;
    }
    &__teaser-title {
      padding: 0 40px;
    }
  }

  @media screen and (max-width: 768px) {
    &__teaser-title {
      padding: 0 12px;
    }
  }
}
</style>
