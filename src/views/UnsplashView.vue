<template>
  <div>
    <HeaderCont />
    <TitleCont name1="unsplash" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider"></div>
          <div class="unsplash__search">
            <div class="container">
              <h2>검색하기</h2>
              <input
                ref="{{inputRef}}"
                type="search"
                placeholder="검색하세요!"
                onKeyPress="{{onKeyPress}}"
              />
              <button type="submit" onClick="{{onClick}}">검색</button>
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

export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
  },

  setup() {
    const splashes = ref([]);
    const search = ref("landscape");

    const SearchSplashes = () => {
      fetch(
        `https://api.unsplash.com/search/photos/?client_id=sf6Q68YlU2mVvJqWMapJjCgVSQShpVebqPohf5BOb08&query=${search.value}$page=1`
      )
        .then((response) => response.json())
        .then((result) => (splashes.value = result.results))
        .catch((error) => console.log(error));
    };
    SearchSplashes();

    const RandomSplashes = async () => {
      await fetch(
        "https://api.unsplash.com/photos/random?client_id=sf6Q68YlU2mVvJqWMapJjCgVSQShpVebqPohf5BOb08&count=20"
      )
        .then((response) => response.json())
        .then((result) => (splashes.value = result))
        .catch((error) => console.log(error));
    };
    RandomSplashes();

    return {
      splashes,
      search,
      // SearchSplashes,
      RandomSplashes,
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
</style>
