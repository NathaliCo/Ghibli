<template>
  <section>
    <b-modal id="bv-modal-example" hide-footer>
    <template #modal-title >
    </template>
    <div class="t-busq">
      <img src="./assets/indice.png">
      <p><label for="seleccionado">If you want to know more about ghibli choose an option:</label></p>
	</div>
	<div>
		<center>
		<select v-model="seleccionado" class="">
			<option disabled value="-1">Select an Element</option>
			<option value="people" v-on:click="muestraFormulario($event.target.value)">People</option>
			<option value="locations" v-on:click="muestraFormulario($event.target.value)">Locations</option>
			<option value="species" v-on:click="muestraFormulario($event.target.value)">Species</option>
			<option value="vehicles" v-on:click="muestraFormulario($event.target.value)">Vehicles</option>
		</select>
		</center>
	</div>
    <div class="d-block text-center">
            <span v-if="formulario">
            <div v-if="seleccionado==='people'">
                <select v-model="people.name">
                <option selected="selected" value="all">All</option>
                <option v-for="(elemento) in peoplelist"
                v-bind:key="elemento" v-bind:value="elemento">{{elemento}}</option>
                </select>
            </div>
            <div v-if="seleccionado==='locations'">
              <select v-model="locations.name">
                <option selected="selected" value="all">All</option>
                <option v-for="(elemento) in locationslist"
                v-bind:key="elemento" v-bind:value="elemento">{{elemento}}</option>
                </select>
            </div>
            <div v-if="seleccionado==='species'">
                <select v-model="species.name">
                <option selected="selected" value="all">All</option>
                <option v-for="(elemento) in specieslist"
                v-bind:key="elemento" v-bind:value="elemento">{{elemento}}</option>
                </select>
            </div>
            <div v-if="seleccionado==='vehicles'">
                <select v-model="vehicles.name">
                <option selected="selected" value="all">All</option>
                <option v-for="(elemento) in vehicleslist"
                v-bind:key="elemento" v-bind:value="elemento">{{elemento}}</option>
                </select>
            </div>
              <b-button v-on:click="generaFiltro(seleccionado)" variant="success" class="search">Search</b-button>
              <section v-html="elForm">
              </section>
            </span>
    </div>
    <div >                  
	<b-card-group deck v-for="(actual, indice) in busact" 
				v-bind:key="indice">
		<b-card bg-variant="secondary" text-variant="white" class="text-center mod-card" v-if="busact!=''">
			<b-card-text >
			<h3>{{actual.name}}</h3>
			<p  v-for="(elemento, key) in actual" 
				v-bind:key="elemento.id"
			>
				<span v-if="seleccionado==='people' && people.mostrar.includes(key)">
				<strong>{{key| daFormato}}: </strong> {{elemento}}
				</span>
				<span v-if="seleccionado==='locations' && locations.mostrar.includes(key)">
				<strong>{{key| daFormato}}: </strong> {{elemento}}
				</span>
				<span v-if="seleccionado==='species' && species.mostrar.includes(key)">
				<strong >{{key| daFormato}}: </strong> {{elemento}}
				</span>
				<span v-if="seleccionado==='vehicles' && vehicles.mostrar.includes(key)" >
				<strong >{{key | daFormato}}: </strong> {{elemento}}
				</span>
			</p>
			</b-card-text>
			<b-button onclick="" href="#" variant="primary">See more</b-button>
		</b-card>
	</b-card-group>
  </div>
    <b-button class="mt-3" block @click="$bvModal.hide('bv-modal-example')">Close Me</b-button>
  </b-modal>
    </section>
  
</template>

<script>
import axios from 'axios'
export default {
    props:['peliID' ],
    data(){
        return {
            name: 'modal',
            seleccionado:'',
            elForm:'',
            formulario:false,
            specieslist:[],
            locationslist:[],
            vehicleslist:[],
            peoplelist:[],
            busact:null,
            hayDatos:false,
            people:{name:'all',
                    mostrar: ['gender','age','eye_color','hair_color'],
                    ocultar:['films', 'species']
                    },
            locations:{name:'all',
                      mostrar: ['climate','terrain','surface_water'],
                      ocultar:['residents', 'films']
            },
            vehicles:{name:'all',
                    mostrar: ['description','vehicle_class','length'],
                    ocultar:['pilot', 'films']
                    },
            species:{name:'all',
                    mostrar :['classification', 'eye_colors', 'hair_colors'],
                    ocultar:['people', 'films']
                    },
            filtro:null
        }
    },
    mounted(){
        this.traeInfo(null, 'species');
        this.traeInfo(null, 'locations');
        this.traeInfo(null, 'vehicles');
        this.traeInfo(null, 'people');
		let tfiltro=localStorage.getItem(tfiltro);
		let tlista=localStorage.getItem(tlista);
		if(tfiltro && tlista){
			this.traeInfo(tfiltro, tlista);
		}
    },
    filters: {
      daFormato: function (llave) {
          if (!llave) return ''
          llave = llave.replace('_',' ')
          return llave.charAt(0).toUpperCase() + llave.slice(1)
      }
    },
    methods: {
        traeInfo(filtro, lista){
            axios  
                .get(`https://ghibliapi.herokuapp.com/${filtro?filtro:lista}`)
                .then(response=>{
                    if (filtro===null){
                        (response.data).forEach(element => {
                            lista==='species'?this.specieslist.push(element.name):lista==='vehicles'? this.vehicleslist.push(element.name):lista==='people'? this.peoplelist.push(element.name):this.locationslist.push(element.name)
                        });
                    }else{
						console.log("INFO!");
						localStorage.setItem('tfiltro', filtro);
						localStorage.setItem('tlista', lista);
                        this.busact=response.data
                        this.hayDatos=true;
                    }
                }).catch(e=>console.log(e))
        },
        traeDetalles(lista){
            let respuesta='';
            if (lista.ocultar.isArray()){
              tipo.ocultar.forEach(elemento => {
                axios
                  .get(url)
                  .then(response=>{
                    respuesta=response.data
                  }).catch(e=>console.log(e))
              });
            }else{

            }
        },
        muestraFormulario(tbusqueda){
            this.formulario=true;
			this.busact='';
        },
        generaFiltro(seleccionado){
         	this.busact='';
            switch (seleccionado) {
                case 'people':
                  this.people.name=='' ? this.muestraError(): this.people.name==="all" ? this.filtro="people" : this.filtro = "people?name"+"="+this.people.name;
                    break;
                case 'locations':
                     this.locations.name=='' ? this.muestraError(): this.locations.name==="all" ? this.filtro="locations" : this.filtro = "locations?name"+"="+this.locations.name;
                     break;
                case 'species':
                     this.species.name=='' ? this.muestraError(): this.species.name==="all" ? this.filtro="species" : this.filtro = "species?name"+"="+this.species.name;
                    break;
                case 'vehicles':
                   this.vehicles.name=='' ? this.muestraError(): this.vehicles.name==="all" ? this.filtro="vehicles" : this.filtro = "vehicles?name"+"="+this.vehicles.name;
                    break;
                default:
                    break;
                }
           this.traeInfo(this.filtro);
       },
       muestraError(){
         alert("NOP")
       },
       close() {
        this.$emit('close');
      },
    },
};
</script>

<style >
.t-busq{
  margin:auto;
}
.modal-dialog {
    max-width: 60vw;
}
.mod-card{
  max-width: 70%;
}
.card-deck .card {
  margin: 1rem auto;
}
.search{
  margin: 1rem;
}
.modal-title{
  width:70%;
}
.t-busq{
  display: flex;
  justify-content: center;

}
.t-busq img{
  height: 3rem;
  width: auto;
}
.select{
	margin: 1rem;
}
</style>