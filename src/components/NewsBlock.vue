<template>
  <section class="news">
    <div class="news__container container">
      <ul class="news__list">
        <li
          class="news__item"
          v-for="(item, index) in displayedNews"
          :key="index"
        >
          <img :src="item?.image" alt="" class="news__image" />
          <div class="news__content">
            <div class="news__date">
              <time class="news__date-day">{{ moment.unix(item.date).format("DD") }}</time>
              <div class="news__date-month-year">
                <time class="news__date-month">{{ moment.unix(item.date).format("MMMM") }}</time>
                <time class="news__date-year">{{ moment.unix(item.date).format("YY") }}</time>
              </div>
            </div>
            <h3 class="news__title">{{ item.name }}</h3>
            <p class="news__text">{{ item.previewText }}</p>
            <div class="news__type">{{ item.type.value }}</div>
          </div>
        </li>
      </ul>
      <button v-if="hasMoreNews && !loading" type="button" @click="loadMore" class="news__button">
        Загрузить ещё
      </button>
      <div v-if="loading" class="news__loader">Loading</div>
    </div>
  </section>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import axios from "axios";
import moment from "moment";

export default {
  setup() {
    const newsList = ref([]);
    const currentPage = ref(1);
    const itemsPerPageDesktop = 9;
    const itemsPerPageTablet = 6;
    const itemsPerPageMobile = 3;
    const isTablet = ref(window.innerWidth < 1024 && window.innerWidth >= 768);
    const isMobile = ref(window.innerWidth < 768);
    const itemsPerPage = computed(() => {
      if (isMobile.value) return itemsPerPageMobile;
      if (isTablet.value) return itemsPerPageTablet;
      return itemsPerPageDesktop;
    });
    const loading = ref(false);

    const updateIsTablet = () => {
      isTablet.value = window.innerWidth < 1024 && window.innerWidth >= 768;
      isMobile.value = window.innerWidth < 768;
    };

    window.addEventListener('resize', updateIsTablet);

    const displayedNews = computed(() => {
      const startIndex = 0;
      const endIndex = currentPage.value * itemsPerPage.value;
      return newsList.value.slice(startIndex, endIndex);
    });

    const hasMoreNews = computed(() => displayedNews.value.length < newsList.value.length);

    async function fetchAllNews() {
      loading.value = true;
      try {
        const { data } = await axios.get('https://flems.github.io/test/api/news/');
        const totalPages = data.nav.total;
        const pageRequests = [];

        for (let page = 1; page <= totalPages; page++) {
          pageRequests.push(axios.get(`https://flems.github.io/test/api/news/${page}`));
        }

        const responses = await Promise.all(pageRequests);
        responses.forEach((response) => {
          newsList.value.push(...response.data.items);
        });
      } catch (error) {
        console.error("Failed to fetch news:", error);
      } finally {
        loading.value = false;
      }
    }

    async function loadMore() {
      currentPage.value++;
    }

    onMounted(async () => {
      await fetchAllNews();
    });

    return {
      newsList,
      displayedNews,
      loadMore,
      hasMoreNews,
      loading,
      moment,
    };
  },
};
</script>

<style scoped lang="scss">
.news {
  &__container {
    margin: 0 auto;
  }

  &__list {
    display: flex;
    flex-wrap: wrap;
    gap: 48px;
    margin: 48px 0;
    padding: 0;
    list-style: none;
  }

  @media screen and (max-width: 1024px) {
    &__list {
      gap: 24px;
    }
  }

  &__item {
    flex: 1 1 calc(33.33% - 34px);
    border: 1px solid #0f62fe;
    border-radius: 16px;
    overflow: hidden;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  @media screen and (max-width: 1024px) {
    &__item {
      flex: 1 1 calc(50% - 24px);
    }
  }

  @media screen and (max-width: 768px) {
    &__item {
      flex: 100%;
    }
  }

  &__image {
    max-width: 100%;
    display: block;
    position: relative;
    z-index: 1;
  }

  &__content {
    padding: 16px;
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  &__date {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: fit-content;
    gap: 4px;
  }

  &__date-day {
    font-size: 30px;
    font-weight: 400;
    color: #A1A7B5;
  }

  &__date-month-year {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-right: 36px;
  }

  &__date-month,
  &__date-year {
    font-size: 15px;
    font-weight: 700;
    line-height: 15px;
    color: #A1A7B5;
  }

  &__title {
    font-size: 22px;
    font-weight: 26px;
    font-weight: 400;
    color: #0C5BEF;
    margin: 0;
    margin-top: 16px;
  }

  &__text {
    font-size: 20px;
    line-height: 26px;
    letter-spacing: -1%;
    margin: 0;
    margin-top: 16px;
    margin-bottom: 40px;
  }

  &__type {
    font-size: 14px;
    color: #666;
    display: block;
    margin-top: auto;
    padding: 4px 16px;
    border-radius: 24px;
    background-color: #F0F6FE;
    max-width: fit-content;
  }

  &__button {
    display: block;
    margin: 72px auto;
    padding: 16px 32px;
    background-color: transparent;
    color: #002dbf;
    border: 1px solid #002dbf;
    border-radius: 8px;
    cursor: pointer;
  }

  &__loader {
    margin-top: 10px;
    text-align: center;
  }
}
</style>
