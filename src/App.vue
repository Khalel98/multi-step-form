<template>
  <multiStepForm></multiStepForm>
</template>

<script>
import multiStepForm from "./components/multiStepForm.vue";

export default {
  name: "App",
  components: {
    multiStepForm,
  },
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
