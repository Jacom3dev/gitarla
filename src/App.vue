<script setup>
    import Header from './components/layout/Header.vue';
    import Footer from './components/layout/Footer.vue';
    import Guitar from './components/Guitar.vue';

    import {ref,onMounted} from "vue";
    import {db} from "./data/guitars";

    const guitars = ref([]);

    onMounted(()=>{
        guitars.value = db;
    })
</script>

<template>
 
  <Header/>
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
        <Guitar
            v-for="guitar in guitars"
           :guitar="guitar"
        />
    </div>
  </main>
  <Footer/>

  
</template>

<style scoped>
</style>
