<template>
  <div class="container">
    <h2 class="justify-content-center d-flex">Password Generator</h2>
    <hr />
    <div class="row justify-content-center">
      <user-form></user-form>
    </div>
  </div>
</template>

<script>
import UserForm from "./components/UserForm.vue";

export default {
  components: {
    UserForm
  }
};
</script>

<style></style>
