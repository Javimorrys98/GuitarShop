<script setup>
import { ref, onMounted, watch } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];
  const carritoStorage = localStorage.getItem('carrito')
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }
});


const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

watch(carrito, () => {
  guardarLocalStorage()
}, {
  deep: true
})

const agregarCarrito = (guitarra) => {
  let index = carrito.value.findIndex(g => g.id === guitarra.id);
  if (index != -1) {
    if (carrito.value[index].cantidad < 5) {
      carrito.value[index].cantidad++
    }
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }

  guardarLocalStorage();
};

const borrarGuitarra = (id) => {
  // let index = carrito.value.findIndex(g => g.id === id)
  // if(index != -1){
  //   carrito.value.splice(index, 1)
  // }

  carrito.value = carrito.value.filter(g => g.id !== id)
}

const decrementarCantidad = (id) => {
  let index = carrito.value.findIndex(g => g.id === id)
  if (index != -1) {
    if (carrito.value[index].cantidad > 0) {
      carrito.value[index].cantidad--
    }
  }
}
const incrementarCantidad = (id) => {
  let index = carrito.value.findIndex(g => g.id === id)
  if (index != -1) {
    if (carrito.value[index].cantidad < 5) {
      carrito.value[index].cantidad++
    }
  }
}

const vaciarCarrito = () => {
  carrito.value = []
}
</script>
<template>
  <Header :carrito="carrito" :guitarra="guitarra" @borrar-guitarra="borrarGuitarra"
    @incrementar-cantidad="incrementarCantidad" @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito" @vaciar-carrito="vaciarCarrito"></Header>
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito"></Guitarra>
    </div>
  </main>
  <Footer></Footer>
</template>

<style scoped></style>
