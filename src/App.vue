<script setup>
  import Header from './components/layout/Header.vue';
  import Footer from './components/layout/Footer.vue';
  import Guitar from './components/Guitar.vue';

  import {ref,onMounted,watch} from "vue";
  import {db} from "./data/guitars";

  const guitars = ref([]);
  const cart = ref([]);
  const guitar = ref({});

  watch(cart,()=>{
    setLocalStorage()
  },{
    deep:true
  })
  
  const ramdon = Math.floor(Math.random()*db.length);
  
  onMounted(()=>{
    guitars.value = db;
    guitar.value = db[ramdon];
    const getCart = JSON.parse(localStorage.getItem('cart')) || [];
    cart.value = getCart;
  })

  const setLocalStorage = ()=>{
    localStorage.setItem('cart',JSON.stringify(cart.value));
  }
  
  const addToCart = (guitar)=>{
    const exist = cart.value.find((c)=>c.id === guitar.id);
    if (exist) {
      cart.value.map((c)=>{
        if (c.id == guitar.id) {
          c.cantidad++
        }
      })
    }else{
      cart.value.push({...guitar,cantidad:1});
    }
    setLocalStorage()
  }

  const increment = (id)=> cart.value.find((c)=>c.id===id).cantidad++;
  
  const decrement = (id)=> cart.value.find((c)=>c.id===id).cantidad--;

  const deleteProduct = (id)=>{
    cart.value = cart.value.filter((c)=>c.id !== id);
  }

  const reset = ()=>{
    cart.value = [];
  }



</script>

<template>
 
  <Header
    :guitar="guitar"
    :cart="cart"
    @add="addToCart"
    @increment="increment"
    @decrement="decrement"
    @delet-product="deleteProduct"
    @reset="reset"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
        <Guitar
            v-for="guitar in guitars"
            :key="guitar.id"
           :guitar="guitar"
           @add="addToCart"
        />
    </div>
  </main>
  <Footer/>

  
</template>

<style scoped>
</style>
