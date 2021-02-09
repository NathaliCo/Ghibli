<template>
	<section> <encabezado></encabezado>
	<div id="app" class="container">
	<div class = "">
    	<splash v-if="isLoading"></splash>
		<listados></listados>
    </div>
  </div>
</section>
</template>

<script>
import Listados from './Listados.vue'
import Encabezado from './Encabezado.vue'
import Splash from './Splash.vue'
import Vue from 'vue';
import VueSplash from 'vue-splash';
Vue.use(VueSplash);
export default {
	components: { Encabezado, Listados,  Splash },
	data () {
		return  {
		isLoading: true
		}
  	},
   mounted () {
    	setTimeout(() => {
      	this.isLoading = false
    }, 3000)
  }
}
</script>