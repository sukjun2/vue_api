<template>
  <div>
    <HeaderCont />
    <TitleCont name1="movie" name2="reference api" />
    <section class="cont__movie">
      <div class="container">
        <div class="movie__inner">
          <div class="movie__slider">
            <div class="container">
              <div class="slider__inner">
                <h2>Popular Movies</h2>
                <swiper
                  :effect="'coverflow'"
                  :grabCursor="true"
                  :centeredSlides="true"
                  :slidesPerView="'auto'"
                  :initialSlide="5"
                  :autoplay="{
                    delay: 2500,
                    disableOnInteraction: false,
                  }"
                  :coverflowEffect="{
                    rotate: 50,
                    stretch: 0,
                    depth: 100,
                    modifier: 1,
                    slideShadows: false,
                  }"
                  :pagination="false"
                  :modules="modules"
                  class="mySwiper"
                >
                  <swiper-slide
                    v-for="(slider, index) in sliders"
                    :key="slider.id"
                  >
                    <li>
                      <a
                        :href="`https://www.themoviedb.org/movie/${slider.id}`"
                      >
                        <img
                          :src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`"
                          :alt="slider.title"
                        />
                        <em>
                          <span class="title">{{ slider.title }}</span>
                          <span class="star">{{ slider.vote_average }}</span>
                          <span class="rank">{{ index + 1 }}</span>
                        </em>
                      </a>
                    </li>
                  </swiper-slide>
                </swiper>
              </div>
            </div>
          </div>
          <!-- //movie__slider -->
          <div class="movie__search">
            <div class="container">
              <form @submit.prevent="SearchMoives()">
                <input
                  :ref="search"
                  :type="search"
                  placeholder="검색하세요!"
                  v-model="search"
                />
                <button type="submit">검색</button>
              </form>
            </div>
            <!-- //movie__search -->
          </div>
          <div class="movie__movies">
            <div class="container">
              <div class="movie__list">
                <ul>
                  <li v-for="movie in movies" :key="movie.id">
                    <a :href="`https://www.themoviedb.org/movie/${movie.id}`">
                      <img
                        :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                        :alt="movie.title"
                      />
                      <em>
                        <span class="title">{{ movie.title }}</span>
                        <span class="star">{{ movie.vote_average }}</span>
                      </em>
                    </a>
                  </li>
                </ul>
              </div>
            </div>
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
// swiper
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/effect-coverflow";
import "swiper/css/pagination";
import { EffectCoverflow, Pagination, Autoplay } from "swiper";

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
    const movies = ref([]);
    const sliders = ref([]);
    const search = ref("marvel");

    const SearchMoives = async () => {
      await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=c7b716a0f422919939537929e105303f&query=${search.value}&page=1`
      )
        .then((response) => response.json())
        .then((result) => {
          movies.value = result.results;
          search.value = "";
        })
        .catch((error) => console.log(error));
    };
    SearchMoives();

    const TopMovies = async () => {
      await fetch(
        "https://api.themoviedb.org/3/movie/popular?api_key=c7b716a0f422919939537929e105303f&language=ko-KR&page=1"
      )
        .then((response) => response.json())
        .then((result) => (sliders.value = result.results))
        .catch((error) => console.log(error));
    };
    TopMovies();

    return {
      movies,
      sliders,
      search,
      SearchMoives,
      modules: [EffectCoverflow, Pagination, Autoplay],
    };
  },
};
</script>

<style lang="scss">
.movie__list {
  ul {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

    li {
      width: 23%;
      position: relative;
      transition: all 0.3s ease-in-out;

      &:hover {
        transform: scale(1.05);
      }

      em {
        display: block;
        height: 80px;
        margin-bottom: 30px;
        font-family: var(--font-sub2);
      }
      .title {
        padding: 5px 0;
        display: inline-block;
      }
      .star {
        background: #fff;
        color: #000;
        position: absolute;
        left: 10px;
        top: 10px;
        width: 30px;
        height: 30px;
        border-radius: 100px;
        text-align: center;
        line-height: 30px;
        font-weight: 800;
      }
    }
  }
}

.movie__search {
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

.movie__slider {
  margin-bottom: 50px;

  h2 {
    color: var(--black);
    font-family: var(--font-sub2);
  }

  .slider__inner {
    .swiper {
      width: 100%;
      padding-top: 50px;
      // padding-bottom: 50px;
    }

    .swiper-slide {
      background-position: center;
      background-size: cover;
      width: 300px;
      height: 500px;

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
          em {
            display: block;
            margin-bottom: 20px;
            font-family: var(--font-sub2);

            .title {
              padding: 5px 0;
              display: inline-block;
            }
            .star {
              background: #fff;
              color: #000;
              position: absolute;
              right: 15px;
              top: 15px;
              width: 30px;
              height: 30px;
              border-radius: 100px;
              text-align: center;
              line-height: 30px;
              font-weight: 800;
            }
            .rank {
              background: repeating-linear-gradient(
                135deg,
                rgb(255, 221, 155) 0,
                rgb(254, 215, 146) 20px,
                rgb(255, 225, 172) 40px,
                rgb(255, 255, 255) 70px,
                rgb(250, 198, 107) 125px,
                rgb(246, 194, 107) 160px,
                rgb(231, 177, 85) 175px,
                rgb(244, 193, 107) 185px,
                rgb(241, 191, 107) 220px,
                rgb(243, 190, 102) 235px,
                rgb(233, 177, 83) 250px,
                rgb(245, 190, 97) 280px,
                rgb(249, 188, 86) 305px,
                rgb(255, 251, 245) 385px,
                rgb(252, 195, 98) 415px,
                rgb(252, 195, 121) 450px,
                rgb(255, 221, 155) 500px
              );
              background-clip: text;
              -webkit-background-clip: text;
              background-repeat: repeat;
              background-attachment: fixed;
              backface-visibility: none;
              color: rgb(255, 221, 155);
              -webkit-text-fill-color: transparent;
              filter: drop-shadow(0 3px 1px #000);
              position: absolute;
              font-size: 40px;
              left: 10px;
              top: 0px;
              font-family: var(--font-main);
              font-weight: 800;
            }
          }
        }
      }
    }
  }
}
</style>
