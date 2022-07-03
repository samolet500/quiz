<template>
  <div class="home">
    <div class="container">
      <a class="reset-link" href="#" @click.prevent="onClearLS">Oчистить local Storage, чтобы показать опросник при следующей загрузке</a>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
// import Quiz from "@/components/Quiz.vue";

export default {
  name: 'Home',

  methods: {
    onClearLS() {
      localStorage.removeItem('hide-quiz');
    }
  }
}
</script>

<style lang="scss">
.reset-link {
  display: inline-block;
  margin-top: 100px;
  text-decoration: none;
  color: #000;
  &:hover {
    color: #0FA1DB;
  }
}
</style>
