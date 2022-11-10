<template>
  <div>
    <HeaderCont />
    <TitleCont name1="unsplash" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider"></div>
          <div class="unsplash__search"></div>
          <div class="unsplash__images">
            <ul>
              <li v-for="splash in splashes" :key="splash.id">
                <a href="#">
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

    // const SearchSplashes = () => {
    //   fetch(
    //     `https://api.unsplash.com/search/photos/?client_id=sf6Q68YlU2mVvJqWMapJjCgVSQShpVebqPohf5BOb08&query=${search.value}`
    //   )
    //     .then((response) => response.json())
    //     .then((result) => console.log(result))
    //     .catch((error) => console.log(error));
    // };
    // SearchSplashes();

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
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    li {
      width: 19%;
      margin-bottom: 2%;
    }
  }
}
</style>
