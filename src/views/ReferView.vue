<template>
  <div>
    <HeaderCont />
    <TitleCont name1="reference" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="refer__inner">
          <h2>CSS</h2>
          <ul class="refer__list">
            <li v-for="refer in refers" :key="refer.title">
              <a href="#">
                <span class="num">{{ refer.num }}</span>
                <span class="name">{{ refer.title }}</span>
                <span class="desc">{{ refer.desc }}</span>
                <span class="star">{{ refer.descStar }}</span>
              </a>
            </li>
          </ul>
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

export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
  },

  setup() {
    const refers = ref([]);

    const references = () => {
      fetch("https://sukjun2.github.io/react_api/src/utils/reference.json")
        .then((response) => response.json())
        // .then((result) => console.log(result.cssRefer))
        .then((result) => (refers.value = result.cssRefer))
        .catch((error) => console.log("error", error));
    };
    references();

    return {
      refers,
      references,
    };
  },
};
</script>

<style lang="scss">
.refer__inner {
  font-family: "GangwonEduSaeeum-Display";
  padding-bottom: 200px;

  h2 {
    font-size: 60px;
    color: var(--black);
  }
}

.refer__list {
  border: 1px solid var(--bg-dark-border);
  font-size: 20px;

  li {
    border-bottom: 1px solid var(--bg-dark-border);
    a {
      display: flex;
      align-items: center;
      width: 100%;
      color: var(--black);

      span {
        display: inline-block;
        padding: 15px 20px;
      }
      .num {
        flex: 1 1 5%;
        text-align: center;
        border-right: 1px solid var(--bg-dark-border);
      }
      .name {
        flex: 1 1 20%;
        border-right: 1px solid var(--bg-dark-border);
      }
      .desc {
        flex: 1 1 60%;
        border-right: 1px solid var(--bg-dark-border);
      }
      .star {
        flex: 1 1 15%;
        text-align: center;
      }
    }
  }
}
</style>
