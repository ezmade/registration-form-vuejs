<template>
    <div id="app">
        <div class=registration-wrapper>
            <RegistrationForm />
        </div>
    </div>
</template>

<script>
    import RegistrationForm from './components/RegistrationForm.vue'

    export default {
        name: "App",
        components: {
            RegistrationForm
        }
    };
</script>

<style>
    * {
        padding: 0;
        margin: 0;
    }

    html {
        scroll-behavior: auto;
        font-size: 62.5%;  /* 1 rem = 10 px */
    }

    #app {
        height: 100vh;
        width: 100vw;
    }

    .registration-wrapper {
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgb(176, 230, 255);
    }

    .registration-form {
        height: 90vh;
        width: 80vw;
        background-color: rgb(250, 250, 250);
        border-radius: 1.5rem;
    }
</style>
