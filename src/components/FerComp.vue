<script setup>
import { reactive, computed } from 'vue'

// ──────────────────────────────────────────────
// ESTADO REACTIVO (Rol A)
// ──────────────────────────────────────────────

const tienda = reactive({
  carrito: []
})

const cliente = reactive({
  nombre: '',
  email: ''
})

const catalogo = [
  { id: 1, nombre: 'Pokeball', precio: 150 },
  { id: 2, nombre: 'Pocion', precio: 50 },
  { id: 3, nombre: 'Baya', precio: 30 },
  { id: 4, nombre: 'SuperBall', precio: 200 },
]

function agregarAlCarrito(producto) {
  const existe = tienda.carrito.find(item => item.id === producto.id)
  if (existe) {
    existe.cantidad++
  } else {
    tienda.carrito.push({ ...producto, cantidad: 1 })
  }
}

function quitarDelCarrito(id) {
  const index = tienda.carrito.findIndex(item => item.id === id)
  if (index !== -1) {
    tienda.carrito.splice(index, 1)
  }
}

function cambiarCantidad(id, delta) {
  const item = tienda.carrito.find(item => item.id === id)
  if (item) {
    item.cantidad = Math.max(1, item.cantidad + delta)
  }
}

// ──────────────────────────────────────────────
// COMPUTED (Rol A los expone, Rol B los muestra)
// ──────────────────────────────────────────────

const totalItems = computed(() => {
  return tienda.carrito.reduce((sum, item) => sum + item.cantidad, 0)
})

const subtotal = computed(() => {
  return tienda.carrito.reduce((sum, item) => sum + item.precio * item.cantidad, 0)
})

// Regla de descuento: 10% si subtotal >= 50.000
const descuento = computed(() => {
  return subtotal.value >= 50000 ? subtotal.value * 0.10 : 0
})

const total = computed(() => {
  return subtotal.value - descuento.value
})

// Validaciones del cliente
const emailValido = computed(() => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(cliente.email)
})

const puedeComprar = computed(() => {
  return cliente.nombre.trim() !== '' && emailValido.value && tienda.carrito.length > 0
})
</script>

<template>
    <nav>
        <ol class="steps">
            <li :class="{ on: paso >= 1 }">Carrito</li>
            <li :class="{ on: paso >= 2 }">Datos</li>
            <li :class="{ on: paso >= 3 }">Confirmar</li>
        </ol>
    </nav>
    <section class="main">
        <article>
            <h1>Carrito de Compra: PokéMart</h1>

            <h2>Catálogo</h2>
            <div class="producto" v-for="prod in catalogo" :key="prod.id">
                <p>{{ prod.nombre }} <strong>{{ prod.precio }}</strong></p>
                <button @click="agregarAlCarrito(prod)">+ Agregar</button>
            </div>

            <h2>Carrito</h2>
            <p>El carrito está vacío, entrenador. ¡Busca algún producto!</p>
            <!--<h4>{{ item.nombre }} × {{ item.cantidad }} <strong @click="quitarDelCarrito(item.id)"></strong></h4>-->
            <div class="carrito">
                <p>Subtotal <strong>$5000</strong></p>
                <p>Descuento <strong>$5000</strong></p>
                <h3>TOTAL <strong>$5000</strong></h3>
                <button>+ Agregar</button>
            </div>
        </article>

        <aside>
            <h2>Tus datos</h2>

            <label>Nombre</label>
            <input type="text">

            <label>E-mail</label>
            <input type="text">
        </aside>
    </section>
    

</template>

<style scoped>
    nav {
        width: 100%;
        padding: 4%;
        background-color: red;
        color: white;
    }

    section.main {
        width: 100%;
        padding: 2%;
        border-radius: 10px;
        border: 2px solid red;
        display: flex;
    }

    article {
        width: 60%;
        padding: 2%;
        background-color: white;
    }

    .producto {
        width: 98%;
        padding: 1%;
        border: 1px solid black;
        display: flex;
    }

    .producto p {
        width: 70%;
        text-align: left;
    }

    .producto p strong {
        padding-left: 5%;
        padding-right: 2%;
        float: right;
    }

    .producto button {
        width: 15%;
        padding: 2%;
    }

    .carrito {
        width: 98%;
        padding: 1%;
        border-radius: 10px;
        background-color: black;
        color: white;
    }

    aside {
        width: 30%;
        background-color: white;
        border: 2px solid black;
        padding: 2%;
    }
</style>