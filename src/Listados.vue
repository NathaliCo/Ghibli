<template>
    <section>
        <div v-if="ghibli==true">
            <center>
                <h1>List of movies</h1>
                <div>
                    <b-button v-b-modal.modalPopover>Help</b-button>
                    <b-modal id="modalPopover" title="" ok-only>
                        <videoh></videoh>
                    </b-modal>
                </div>
                <label>Order movies by  </label>
                <input type="radio" id="date" value="release_date" 
                v-model="seleccionado" 
                v-on:click="ordenar($event.target.value)"
                />
                <label for="date">Date</label>
                <input type="radio" id="rate" value="rt_score" 
                v-model="seleccionado" 
                v-on:click="ordenar($event.target.value)"
                />
                <label for="rate">Rate</label>
                <br>
                <input class="buscador" type="text" id="title" placeholder="Search by name" 
                v-on:keyup.enter="filtrar ($event.target.value, 'peliculas', 'title')" 
                />
            </center>
            <div class="lista-pelis">
                <div v-for="(pelicula, indice) in (filtrado==true? pelisFiltradas:peliculas)" 
                v-bind:key="indice"
                >
                    <pelicula v-bind:laPelicula= pelicula>
                    </pelicula>
                </div>
            </div>
            <center>
                <b-button v-on:click="cambiar()" class="btn-gris">Do you want more anime? click me!</b-button>
            </center>
        </div>
        <center v-if="ghibli==false">
            <div class="formulario">
                <label>Search anime by </label>
                <input type="radio" id="text" value="text" 
                v-model="seleccionado"
                />
                <label for="text">Title</label>
                <input type="radio" id="categories" value="categories" 
                v-model="seleccionado"
                />
                <label for="categories">Category</label>
            </div>
            <div v-if="seleccionado=='text'">
                 <input v-model="filtro" class="buscador" type="text" id="title" placeholder="Search by title" 
                 v-on:keyup.enter="traeAnime(filtro, seleccionado)" 
                 />
            </div>
            <div v-if="seleccionado=='categories'">
                <select v-model="filtro">
                    <option v-for="(elemento) in categorias"
                    v-bind:key="elemento" 
                    v-bind:value="elemento"
                    >
                    {{elemento}}</option>
                </select>
                <b-button v-on:click="traeAnime(filtro, seleccionado)" variant="success" class="search">Search</b-button>
            </div>
            <b-card-group deck v-for="(actual, indice) in busact" 
            v-bind:key="indice"
            >
                <b-card
                v-bind:title="actual.attributes.titles.en"
                v-bind:img-src="actual.attributes.posterImage.large"
                v-bind:img-alt="actual.attributes.titles.en"
                style="max-width: 100%;"
                class="mb-2"
                >
                    <b-card-text class="card-texto">
                        <h4></h4>
                        <div class='datos-peli'> 
                            <div>Started: {{actual.attributes.startDate}}</div>
                            <div> Status: {{actual.attributes.status}}</div>
                            <div>Rating: {{actual.attributes.ratingRank}}</div>
                        </div>
                    <p class='desc-peli'> {{actual.attributes.description}}</p>
                    </b-card-text>
                 </b-card>
	        </b-card-group>
            <b-button v-on:click="cambiar()" class="btn-gris">Back to ghibli</b-button>
        </center>
    </section>
</template>

<script>
import axios from 'axios'
import Pelicula from './Pelicula.vue';
import Videoh from './Videoh.vue';
export default {
  components: { Pelicula, Videoh },
    data(){
        return{
            peliculas:null,
            seleccionado:"",
            pelisFiltradas:[],
            laPelicula:null,
            filtrado:false,
            ghibli:true,
            imagenes:[
            {
                id:"2baf70d1-42bb-4437-b551-e5fed5a87abe",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/castillo-en-el-cielo-studio-ghibli-2-1545138641.png"
            
            },
            {
                id:"12cfb892-aac0-4c5b-94af-521852e46d6a",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/tumba-de-las-luciernagas-studio-ghibli-5-1545138683.png",
            },
            {
                id:"58611129-2dbc-4a81-a72f-77ddfc1b1b49",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/mi-vecino-totoro-studio-ghibli-5-1545138723.jpg",
            },
            {
                id:"ea660b10-85c4-4ae3-8a5f-41cea3648e3e",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/kiki-aprendiz-de-bruja-studio-ghibli-5-1545138765.jpg",
            },
            {
                id:"4e236f34-b981-41c3-8c65-f8c9000b94e7",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/recuerdos-del-ayer-studio-ghibli-6-1545138800.jpg",
            },
            {
                id:"ebbb6b7c-945c-41ee-a792-de0e43191bd8",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/porco-rosso-studio-ghibli-6-1545138858.jpg",
            },
            {
                id:"1b67aa9a-2e4a-45af-ac98-64d6ad15b16c",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/pompoko-studio-ghibli-4-1545138894.jpg",
            },            
            {
                id:"ff24da26-a969-4f0e-ba1e-a122ead6c6e3",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/susurros-del-corazon-studio-ghibli-1-1545138927.jpg",
            },
            {
                id:"0440483e-ca0e-4120-8c50-4c8cd9b965d6",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/princesa-mononoke-studio-ghibli-3-1545138963.jpg",
            },            
            {
                id:"45204234-adfd-45cb-a505-a8e7a676b114",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/mis-vecinos-los-yamada-studio-ghibli-1-1545138999.jpg",
            },
            {
                id:"dc2e6bd1-8156-4886-adff-b39e6043af0c",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/el-viaje-de-chihiro-studio-ghibli-3-1545139045.png",
            },            
            {
                id:"90b72513-afd4-4570-84de-a56c312fdf81",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/haru-reino-de-los-gatos-studio-ghibli-2-1545139082.jpg",
            },
            {
                id:"cd3d059c-09f4-4ff3-8d63-bc765a5184fa",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/el-castillo-ambulante-studio-ghibli-2-1545139121.png",
            },            
            {
                id:"112c1e67-726f-40b1-ac17-6974127bb9b9",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/cuentos-de-terramar-studio-ghibli-3-1545139153.jpg",
            },
            {
                id:"758bf02e-3122-46e0-884e-67cf83df1786",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/ponyo-en-el-acantilado-studio-ghibli-1-1545139185.jpg",
            },            
            {
                id:"2de9426b-914a-4a06-a3a0-5e6d9d3886f6",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/arrietty-studio-ghibli-1-1545139217.jpg",
            },
            {
                id:"45db04e4-304a-4933-9823-33f389e8d74d",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/la-colina-de-las-amapolas-studio-ghibli-1-1545139245.jpg",
            },            
            {
                id:"67405111-37a5-438f-81cc-4666af60c800",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/el-viento-se-levanta-studio-ghibli-1-1545139288.jpg",
            },            
            {
                id:"578ae244-7750-4d9f-867b-f3cd3d6fecf4",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/el-cuento-de-la-princesa-kaguya-studio-ghibli-2-1545139315.jpg",
            },            
            {
                id:"5fdfb320-2a02-49a7-94ff-5ca418cae602",
                img:"https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/el-recuerdo-de-marnie-studio-ghibli-1-1545139346.jpeg",
            }
        ],
        filtro:'',
        categorias:'',
        busact:''
       }
    },
    mounted(){
        this.traePeliculas();
        this.traeCategorias();
        let filtro= localStorage.getItem('filtro');
        let tipo=localStorage.getItem('tipo');
        let orden=localStorage.getItem('orden');
        if(filtro && tipo){
            this.traeAnime(filtro, tipo);
        }
        if(orden){
            setTimeout(() => {
                this.ordenar(orden);
            }, 3000)            
        }

    },
    methods: {
        traePeliculas(){
            axios  
                .get('https://ghibliapi.herokuapp.com/films/')
                .then(response=>{
                    let imagenes= this.imagenes;
                    let peliculas =response.data
                    peliculas.forEach(function(pelicula, indice) {
                        imagenes.find(function(elemento){
                            if (elemento.id==pelicula.id){
                                 peliculas[indice].img=elemento.img;
                             }
                        })
                        this.peliculas=peliculas
                    }.bind(this));
                }).catch(e=>console.log(e))
        },
        ordenar(filtro){
            this.peliculas.sort(function(a,b){
               return (a[filtro] - b[filtro])
            })
            localStorage.setItem('orden', filtro);
        },        
        filtrar (cadena, lista, filtro){
            this.filtrado=true
            let pelisFiltradas=[];
            lista='peliculas'? lista=this.peliculas:lista="";
            const found = lista.find(function(peli) {
              if(peli[filtro].toLowerCase().includes(cadena.toLowerCase())){
                  pelisFiltradas.push(peli);
              }
            })
            this.pelisFiltradas=pelisFiltradas
        },
        cambiar(){
           
            this.ghibli==true?this.ghibli=false:this.ghibli=true;
        },
        traeAnime(filtro, tipo){
                let filtroBusq=`?filter[${tipo}]=${filtro}`;
             axios  
                .get(`https://kitsu.io/api/edge/anime${filtroBusq}`)
                .then(response=>{
                    this.busact=response.data.data
                    localStorage.setItem('filtro', filtro);
                    localStorage.setItem('tipo', tipo);
                }).catch(e=>console.log(e))
        },
        traeCategorias(){
        let categorias=[];
        axios  
            .get(`https://kitsu.io/api/edge/anime/942/categories`)
            .then(response=>{
                (response.data.data).forEach(element => {
                    categorias.push(element.attributes.title)
                });
                this.categorias=categorias;
            }).catch(e=>console.log(e))
         }
    },
}
</script>

<style scoped>
.lista-pelis{
    margin: 2rem auto;
    width:70%;
}
.buscador{
    width:50%;
}
.card{
    max-width: 40% !important;
}
.btn-gris{
    margin:1rem 0 5rem;
}
</style>