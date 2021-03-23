<template>
  <div class="wrapper" v-bind:class="[{ top: statement.isData }, wrapper]">
    <WelcomeText v-if="statement.show == true" />
    <searchForm v-model:inputValue="inputValue" @handleClick="getResults" />
    <p v-if="statement.loading">Loadnig...</p>
  </div>
</template>

<script>
const baseUrl = "https://pokeapi.co/api/v2/pokemon/";
import { ref, reactive } from "vue";
import WelcomeText from "./components/WelcomeText.vue";
import SearchForm from "./components/SearchForm.vue";
export default {
  components: { WelcomeText, SearchForm },
  name: "App",

  setup() {
    const inputValue = ref("");
    const pokeItem = ref({});
    const statement = reactive({
      show: true,
      isData: false,
      loading: false,
    });

    const getResults = async () => {
      statement.loading = true;
      try {
        const response = await fetch(`${baseUrl}${inputValue.value}`);
        const data = await response.json();

        console.log(data);

        pokeItem.value = {
          name: data.name,
          img: data.sprites.front_default,
          weight: data.weight,
          typesAsText: data.types.map(({ type: { name } }) => name).join(", "),
        };

        statement.show = false;
        statement.isData = true;
        statement.loading = false;
      } catch {
        alert("error");
      }
    };

    return {
      inputValue,
      getResults,
      pokeItem,
      statement,
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.wrapper {
  width: 100%;
  height: 100vh;
  padding: 40px 0 40px 0;
  background: url("./assets/welcome.jpg");
  background-position: right;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.top {
  background: none;
  justify-content: flex-start;
}
</style>
