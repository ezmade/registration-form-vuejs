<template>
  <div id="app">
    <div class="registration-wrapper">
      <RegistrationForm />
    </div>
  </div>
</template>

<script>
import RegistrationForm from "./components/RegistrationForm.vue";

export default {
  name: "App",
  components: {
    RegistrationForm,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700;1,900&display=swap");
* {
  padding: 0;
  margin: 0;
}

html {
  scroll-behavior: auto;
  font-size: 62.5%; /* 1 rem = 10 px */
  font-family: 'Source Sans Pro', sans-serif; 
}

#app {
  height: fit-content;
  min-height: 100vh;
  width: 100vw;
}

.registration-wrapper {
  height: 100%;
  min-height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(176, 230, 255);
}
</style>
