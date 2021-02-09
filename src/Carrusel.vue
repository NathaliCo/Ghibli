<template>
	<div class="elCarrusel">
		<b-carousel
		id="carousel-1"
		v-model="slide"
		:interval="4000"
		controls
		indicators
		background="#ababab"
		style="text-shadow: 1px 1px 2px #333;"
		@sliding-start="onSlideStart"
		@sliding-end="onSlideEnd"
		>
			<b-carousel-slide img-src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/mi-vecino-totoro-studio-ghibli-1-1545138718.jpg">
			</b-carousel-slide>

			<b-carousel-slide img-src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/nausica-de-la-valle-del-viento-studio-ghibli-4-1545138586.jpg">
			</b-carousel-slide>

			<b-carousel-slide img-src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/castillo-en-el-cielo-studio-ghibli-1-1545138638.jpg">
			</b-carousel-slide>
			
			<b-carousel-slide img-src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/tumba-de-las-luciernagas-studio-ghibli-6-1545138683.png">
			</b-carousel-slide>

			<b-carousel-slide img-src="https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/kiki-aprendiz-de-bruja-studio-ghibli-3-1545138765.png">
			</b-carousel-slide>
		</b-carousel>
	</div>
</template>

<script>
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import { BCarousel } from 'bootstrap-vue'
export default {
	components: { BCarousel},
	data() {
		return {
			slide: 0,
			sliding: null
		}
	},
	methods: {
		onSlideStart(slide) {
			this.sliding = true
		},
		onSlideEnd(slide) {
			this.sliding = false
		}
	}
}
</script>

<style>
.elCarrusel, .carousel-item{
	height: 60vh;
}
</style>