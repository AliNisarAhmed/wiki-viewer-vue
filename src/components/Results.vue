<template>
  <transition name="result">
    <a class="result" :href="`https://en.wikipedia.org/wiki?curid=${result.pageid}`" target="_blank">
      <h3 class="result-title">{{ result.title }}</h3>
      <p class="result-text" v-html="result.snippet"></p>
    </a>
  </transition>
</template>

<script>
export default {
  name: "Results",
  props: ["result", "index"],
}
</script>

<style>

</style>