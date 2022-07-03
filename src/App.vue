<template>
  <div id="app">
    <header-nav/>
    <router-view/>

    <modal-window
        ref="modalQuiz"
        @onAfterModalClose="onCloseQuiz">
      <quiz @quizFinish="onQuizFinish"></quiz>
    </modal-window>
  </div>
</template>

<script>
import HeaderNav from "@/components/HeaderNav.vue";
import ModalWindow from "@/components/ModalWindow";
import Quiz from "@/components/Quiz.vue";

export default {
  name: "App",

  components: {
    ModalWindow,
    HeaderNav,
    Quiz
  },

  methods: {
    onQuizFinish() {
      this.$refs.modalQuiz.closeModal()
    },
    onCloseQuiz() {
      localStorage.setItem('hide-quiz', true);
    }
  },

  mounted() {
    if (!localStorage.getItem('hide-quiz')) {
      this.$refs.modalQuiz.openModal()
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@100;200;300;400;500;600;700&family=Roboto:wght@100;300;400;500;700;900&display=swap");

html {
  margin: 0;
}

body {
  margin: 0;
  font-family: "Roboto Mono", monospace;

  * {
    box-sizing: border-box;
  }
}

.container {
  max-width: 1280px;
  margin: 0 auto;
}

.btn {
  padding: 10px 50px;
  background: #4bbdfd;
  border-radius: 5px;
  border: 1px solid #4bbdfd;
  font-weight: 700;
  font-size: 14px;
  line-height: 18px;
  color: #fff;
  cursor: pointer;
  transition: 0.2s;

  &:hover {
    background: #14aafe;
  }
}

textarea {
  width: 260px;
  min-height: 75px;
  margin-bottom: 12px;
  resize: none;
  padding: 10px;
  background: #f5f5f5;
  border: 2px solid #b3b3b3;
  border-radius: 5px;
  font-weight: 400;
  font-size: 14px;
  line-height: 18px;

  &:disabled {
    background: #E6E6E6;
  }
}

.form-field {
  margin-bottom: 12px;

  &:last-child {
    margin-bottom: 0;
  }
}

.loading-block {
  position: relative;
  z-index: 2;

  &.is-fetch {
    &:before {
      content: '';
      display: block;
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0;
      top: 0;
      background: #fff;
      z-index: 10;
      opacity: .8;
    }
  }

  .preloader {
    position: absolute;
    left: calc(50% - 50px);
    top: calc(50% - 50px);
    z-index: 11;
  }
}
</style>
