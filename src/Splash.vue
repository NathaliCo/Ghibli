<template>
    <div id = "splash">
        <vue-splash
        :show="true"
        :logo="logo"
        title="Studio Ghibli Web"
        color="#00bfa5"
        :size="400"
        :fixed="true"
        />
    </div>
</template>
<script>
import Vue from 'vue';
import VueSplash from 'vue-splash';
 Vue.use(VueSplash);
import Logo from "./assets/logo.png";

export default {
  name: "Splash",
    computed: {
        logo() {
        return Logo;
        }
    },
    methods:{
  },
}
</script>
