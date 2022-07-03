<template>
  <div class="progress">
    <div class="progress__item"
         v-for="item in count"
         :class="{active: current >= item }"
    >
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProgressComponent',

  props: ['count', 'current']
}
</script>

<style scoped lang="scss">
.progress {
  display: flex;
  align-items: center;

  &__item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: relative;
    width: 100%;

    &:first-child {
      width: 21px;
      z-index: 1;
      &:before {
        display: none;
      }
    }

    &.active {
      &:before {
        background: #4BBBFA;
      }

      &:after {
        background: #8CD6FF;
        border: 2px solid #4BBBFA;
      }
    }

    &:before {
      content: '';
      display: block;
      width: 100%;
      height: 3px;
      background: #B3B3B3;
      transition: .3s;
    }

    &:after {
      content: '';
      flex: 0 0 auto;
      display: block;
      width: 21px;
      height: 21px;
      border-radius: 50%;
      background: #C4C4C4;
      border: 2px solid #B3B3B3;
      transition: .3s;
    }
  }
}
</style>