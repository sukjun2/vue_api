<template>
  <div>
    <HeaderCont />
    <TitleCont name1="youtube" name2="reference api" />
    <ContactCont />
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
import FooterCont from "@/components/FooterCont.vue";

export default {
  components: {
    HeaderCont,
    TitleCont,
    ContactCont,
    FooterCont,
  },
};
</script>
