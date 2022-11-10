<template>
  <section class="cont__title">
    <div class="container">
      <h1>
        <strong>{{ name1 }}</strong>
        <em>{{ name2 }}</em>
      </h1>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    name1: String,
    name2: String,
  },
};
</script>

<style lang="scss">
// cont__title
.cont__title {
  padding: 14vmax 0;
  font-size: 80px;
  font-family: var(--font-main);
  color: var(--black);
  line-height: 1;
  text-transform: uppercase;

  strong {
    display: block;
  }
}
</style>
