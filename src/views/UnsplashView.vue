<template>
  <div>
    <HeaderCont />
    <TitleCont name1="unsplash" name2="reference api" />
    <section class="cont__unsplash">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider">
            <div class="container">
              <div class="slider__inner">
                <h2>Unsplash Image</h2>
                <swiper
                  :slidesPerView="'auto'"
                  :centeredSlides="true"
                  :spaceBetween="30"
                  :initialSlide="3"
                  :pagination="{
                    clickable: true,
                  }"
                  :modules="modules"
                  class="mySwiper"
                  :autoplay="{
                    delay: 2500,
                    disableOnInteraction: false,
                  }"
                >
                  <swiper-slide v-for="slider in sliders" :key="slider.id">
                    <li>
                      <a :href="`https://unsplash.com/photos/${slider.id}`">
                        <img :src="slider.urls.regular" :alt="slider.title" />
                      </a>
                    </li>
                  </swiper-slide>
                </swiper>
              </div>
            </div>
          </div>
          <div class="unsplash__search">
            <div class="container">
              <form @submit.prevent="SearchSplashes()">
                <input
                  :ref="search"
                  :type="search"
                  placeholder="검색하세요!"
                  v-model="search"
                />
              </form>
              <button type="submit">검색</button>
            </div>
          </div>
          <div class="unsplash__images">
            <ul>
              <li v-for="splash in splashes" :key="splash.id">
                <a
                  :href="`https://unsplash.com/photos/${splash.id}`"
                  target="_blank"
                >
                  <img :src="splash.urls.regular" :alt="splash.id" />
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>
    <ContactCont />
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import { ref } from "vue";
// import { response } from "express";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/pagination";
import { Pagination, Autoplay } from "swiper";

export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
    Swiper,
    SwiperSlide,
  },

  setup() {
    const splashes = ref([]);
    const sliders = ref([]);
    const search = ref("summer");

    const SearchSplashes = async () => {
      await fetch(
        `https://api.unsplash.com/search/photos/?client_id=sf6Q68YlU2mVvJqWMapJjCgVSQShpVebqPohf5BOb08&query=${search.value}$page=1`
      )
        .then((response) => response.json())
        .then((result) => {
          splashes.value = result.results;
          search.value = "";
        })
        .catch((error) => console.log(error));
    };
    SearchSplashes();

    const RandomSplashes = async () => {
      await fetch("https://sukjun2.github.io/vue_api/src/utils/unsplash.json")
        .then((response) => response.json())
        // .then((result) => console.log(result))
        .then((result) => (sliders.value = result))
        .catch((error) => console.log(error));
    };
    RandomSplashes();

    return {
      splashes,
      sliders,
      search,
      SearchSplashes,
      RandomSplashes,
      modules: [Pagination, Autoplay],
    };
  },
};
</script>

<style lang="scss">
.unsplash__images {
  ul {
    column-count: 4;
    column-gap: 20px;
    width: 100%;

    li {
      margin-bottom: 20px;
      transition: all 0.6s;

      &:hover {
        scale: 1.05;
      }

      img {
        border-radius: 5px;
      }
    }
  }
}

.unsplash__search {
  margin-bottom: 50px;

  .container {
    position: relative;
  }

  h2 {
    color: var(--black);
    font-size: 40px;
    text-indent: -9999px;
    height: 0;
  }
  input {
    background: #fff;
    border: 1px solid var(--black);
    border-radius: 50px;
    color: #000;
    width: 100%;
    padding: 14px 30px;
    font-family: var(--font-sub3);
  }
  button {
    position: absolute;
    right: 10px;
    top: 6px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 0;
    font-family: var(--font-sub3);
    cursor: pointer;
    z-index: 1000;
  }
}

.unsplash__slider {
  margin-bottom: 50px;

  h2 {
    color: var(--black);
    font-family: var(--font-sub2);
    margin-bottom: 10px;
  }

  .slider__inner {
    .swiper {
      width: 100%;
      height: 400px;
      // padding-bottom: 50px;
      border: 1px solid #000;

      .swiper-horizontal > .swiper-pagination-bullets,
      .swiper-pagination-bullets.swiper-pagination-horizontal,
      .swiper-pagination-custom,
      .swiper-pagination-fraction {
        padding-top: 5px;
        border-top: 1px solid #000;
      }

      .swiper-horizontal > .swiper-pagination-bullets .swiper-pagination-bullet,
      .swiper-pagination-horizontal.swiper-pagination-bullets
        .swiper-pagination-bullet {
        background-color: #000;
      }
    }

    .swiper-slide {
      width: 18%;
      text-align: center;
      font-size: 18px;
      display: flex;
      justify-content: center;
      align-items: center;

      li {
        position: relative;
        transition: all 0.3s ease-in-out;
        margin-bottom: 50px;

        &:hover {
          transform: scale(1.05);
        }

        a {
          color: var(--black);

          .swiper-slide img {
            display: block;
            width: 100%;
          }
        }
      }
    }
  }
}
</style>
