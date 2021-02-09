<template>
  <span>
    <center>
    <video ref="myvideo"
    src="./assets/help.mp4"
    width="700"></video>
    <hr>
    <button :disabled="isPlaying" @click="play">Play</button>
    </center>
  </span>
</template>

<script>
export default {
  data(){
    return{
      isPlaying:false,
    }
  },
    methods: {
  	play() {
    	this.$refs.myvideo.play()
      this.isPlaying = true
    },
  }
};
</script>
