<template>
    <section>
        <carrusel></carrusel>
        <div class="logo-div">
            <div>
                <img src="./assets/logo.png"/>
            </div>
            <div>
                <h3>Do you want to become an expert about Ghibli's Universe?</h3>
                <b-button pill variant="outline-secondary" 
                type="button"
                class="btn here"
                id="show-btn" 
                @click="$bvModal.show('bv-modal-example')"
                >Click Here</b-button>
                <hr>
            </div>
            <detalle-modal 
            v-show="isModalVisible"
            @close="closeModal"
            />
        </div>
    </section>
</template>

<script>
import Carrusel from './Carrusel.vue';
import DetalleModal from './DetalleModal.vue'; 

export default {
    components: { DetalleModal, Carrusel},
    data(){
        return{
            titulo:'Ghibli',
            isModalVisible: false
        }
    },
    methods: {
        showModal() {
            this.isModalVisible = true;
        },
        closeModal() {
            this.isModalVisible = false;
        }
    }
}
</script>

<style >
.logo-div{
    display:block;
    text-align:center;
    margin-top:1rem;
}
.here{
    font-size: 1.5rem;
    font-weight: bolder;
}
.here:hover{
    background-color: #716844;
    border-color: #716844;
}
</style>