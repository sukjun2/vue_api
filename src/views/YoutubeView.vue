<template>
  <div>
    <HeaderCont />
    <TitleCont name1="youtube" name2="reference api" />
    <section class="cont__youtube">
      <div class="container">
        <div class="youtube__inner">
          <div class="youtube__slider">
            <div class="container">
              <div class="slider__inner">
                <h2>Youtube</h2>
                <swiper
                  :effect="'cards'"
                  :grabCursor="true"
                  :modules="modules"
                  class="mySwiper"
                  :initialSlide="3"
                  :autoplay="{
                    delay: 2500,
                    disableOnInteraction: false,
                  }"
                >
                  <swiper-slide v-for="slider in sliders" :key="slider.id">
                    <li>
                      <a
                        :href="`https://www.youtube.com/watch?v=${slider.id.videoId}`"
                      >
                        <img
                          :src="slider.snippet.thumbnails.medium.url"
                          :alt="slider.snippet.description"
                        />
                      </a>
                    </li>
                  </swiper-slide>
                </swiper>
              </div>
            </div>
          </div>
          <div class="youtube__search">
            <div class="container">
              <form @submit.prevent="SearchYoutubes()">
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
          <div class="youtube__images">
            <ul>
              <li v-for="youtube in youtubes" :key="youtube.id">
                <img
                  :src="youtube.snippet.thumbnails.high.url"
                  :alt="youtube.snippet.description"
                />
                <em>
                  <span class="title">{{ youtube.snippet.title }}</span>
                </em>
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

import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";

import "swiper/css/effect-cards";
import { EffectCards, Autoplay } from "swiper";

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
    const youtubes = ref([]);
    const sliders = ref([]);
    const search = ref("webstoryboy");

    const SearchYoutubes = async () => {
      await fetch(
        `https://youtube.googleapis.com/youtube/v3/search?part=snippet&q=${search.value}&key=AIzaSyBenl9UdKbF06EZbQpp2Tn-mvbrj3jJvDo&maxResults=20&type=video`
      )
        .then((response) => response.json())
        // .then((result) => console.log(result))
        .then((result) => {
          youtubes.value = result.items;
          search.value = "";
        })
        .catch((error) => console.log(error));
    };
    SearchYoutubes();

    const RandomYoutubes = async () => {
      await fetch("https://sukjun2.github.io/vue_api/src/utils/youtube.json")
        .then((response) => response.json())
        // .then((result) => console.log(result))
        .then((result) => (sliders.value = result.items))
        .catch((error) => console.log(error));
    };
    RandomYoutubes();

    return {
      youtubes,
      sliders,
      search,
      SearchYoutubes,
      RandomYoutubes,
      modules: [EffectCards, Autoplay],
    };
  },
};
</script>

<style lang="scss">
.youtube__search {
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

.cont__youtube {
  ul {
    column-count: 3;
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
      em {
        span {
          overflow: hidden;
          text-overflow: ellipsis;
          display: -webkit-box;
          -webkit-line-clamp: 1;
          -webkit-box-orient: vertical;
          color: #000;
          margin-bottom: 10px;
        }
      }
    }
  }
}

.youtube__slider {
  margin-bottom: 50px;

  h2 {
    color: var(--black);
    font-family: var(--font-sub2);
  }

  .slider__inner {
    .swiper {
      width: 50%;
      padding-top: 50px;
      // padding-bottom: 50px;
    }

    .swiper-slide {
      background-position: center;
      background-size: cover;
      width: 100%;
      border-radius: 20px;

      li {
        position: relative;
        transition: all 0.3s ease-in-out;

        &:hover {
          transform: scale(1.05);
        }

        a {
          color: var(--black);

          img {
            display: block;
            width: 100%;
          }
        }
      }
    }
  }
}
</style>
