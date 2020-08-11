<template>
  <form class="speed">
    <h2 class="speed__title">Уровень сложности:</h2>
    <label v-for="elem in Object.values(speedSettings)" :key="elem.name">
      <input
        v-if="elem.value === speed"
        type="radio"
        class="speed__input"
        :value="elem.value"
        :checked="true"
        @change="$emit('change', elem.value)"
      />
      <input
        v-else
        type="radio"
        class="speed__input"
        :value="elem.value"
        @change="$emit('change', elem.value)"
      />
      {{ elem.name }}
    </label>
  </form>
</template>

<script>
export default {
  props: {
    speed: Number,
    change: Function,
  },
  data() {
    return {
      speedSettings: {
        low: {
          name: "Легкий",
          value: 1500,
        },
        middle: {
          name: "Средний",
          value: 1000,
        },
        hight: {
          name: "Сложный",
          value: 400,
        },
      },
    };
  },
};
</script>

<style lang="scss" scoped>
.speed {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 200px;
  margin: 0 auto;

  &__title {
    margin: 0 auto;
    padding-top: 10px;
    padding-bottom: 10px;
  }

  &__input {
    margin-left: 50px;
  }
}
</style>
