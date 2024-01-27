<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
  guitarras.value = db;
});

const agregarCarrito = (guitarra) => {
  let index = carrito.value.findIndex(g => g.id === guitarra.id);
  if(index != -1){
    carrito.value[index].cantidad++
  }else{
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};

const borrarGuitarra = (guitarra) => {
  let index = carrito.value.findIndex(g => g.id === guitarra.id)
  if(index != -1){
    carrito.value.splice(index, 1)
  }
}
</script>
<template>
  <Header :carrito="carrito" @borrar-guitarra="borrarGuitarra"></Header>
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      ></Guitarra>
    </div>
  </main>
  <Footer></Footer>
</template>

<style scoped></style>
