<template>
  <form>
    <input
      type="text"
      placeholder="e.g. snorlax"
      :value="inputValue"
      @change="(e) => $emit('update:inputValue', e.target.value)"
    />
    <button @click.prevent="() => $emit('handleClick')">Search</button>
  </form>
</template>

<script>
export default {
  name: "searchForm",

  props: {
    inputValue: {
      type: String,
      required: true,
    },
  },

  emits: ["update:inputValue", "handleClick"],
};
</script>

<style scoped>
form {
  width: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

</style>
