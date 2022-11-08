<template>
  <section class="cont__title">
    <div class="container">
      <h1>
        <strong>{props.title[0]}</strong>
        <em>{props.title[1]}</em>
      </h1>
    </div>
  </section>
</template>

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
