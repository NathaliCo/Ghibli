<template>
<section>
    <b-card
    v-bind:title="laPelicula.tittle"
    v-bind:img-src="laPelicula.img"
    v-bind:img-alt="laPelicula.tittle"
    v-bind:tag="laPelicula.tittle"
    style="max-width: 100%;"
    class="mb-2"
    >
    <b-card-text class="card-texto">
        <h4>{{laPelicula.title}}</h4>
        <div class='datos-peli'> 
            <div>{{laPelicula.director}}</div>
            <div> Year: {{laPelicula.release_date}}</div>
            <div>Score: {{laPelicula.rt_score}}</div>
        </div>
      <p class='desc-peli'> {{laPelicula.description}}</p>
    </b-card-text>
  </b-card>
</section>
    
</template>

<script>
export default {
    data(){
    return {
        laImagen:null
       }
    },
    props: ['laPelicula'],
    filters: {
        daFormato: function (llave) {
            if (!llave) return ''
            llave = llave.replace('_',' ')
            return llave.charAt(0).toUpperCase() + llave.slice(1)
        }
    },
}
</script>

<style>
li{
    text-decoration: none;
}
.datos-peli{
    font-weight: bold;
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
}
h4{
    text-align:center;
    margin:1rem;
}
.card-texto{
    margin:0, 10rem;
}
</style>