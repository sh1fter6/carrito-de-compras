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
  { id: 2, nombre: 'Poción', precio: 50 },
  { id: 3, nombre: 'Baya', precio: 30 },
  { id: 4, nombre: 'SuperBall', precio: 200 },
  { id: 5, nombre: 'Master Ball', precio: 49900 },
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
    <header>
      <img src="../assets/img/logo-pokemart.png" alt="Logo PokéMart">
    </header>

    <nav>
      <h1>Carrito de Compra</h1>
      <ol class="steps">
        <li>Carrito</li>
        <li>Datos</li>
        <li>Confirmar</li>
      </ol>
    </nav>
    <section class="main">
        <article>
            <div class="catalogo-mart">
              <h2>Catálogo</h2>
              <div class="producto" v-for="prod in catalogo" :key="prod.id">
                  <p>{{ prod.nombre }} <strong>{{ prod.precio }}</strong></p>
                  <button @click="agregarAlCarrito(prod)">+</button>
              </div>
            </div>

            <div class="tus-compras">
              <h2>Tu Carrito</h2>
              <p v-if="tienda.carrito.length === 0">El carrito está vacío, entrenador. ¡Busca algún producto!</p>
              <div v-else class="cart">
                <div class="compras">
                  <p v-for="i in tienda.carrito" :key="i.id">{{ i.nombre }} × {{ i.cantidad }} <span>{{ (i.precio * i.cantidad).toLocaleString('es-CL') }}</span> <button @click="quitarDelCarrito(tienda.carrito.id)">-</button></p>
                </div>
              </div>
            </div>
        </article>

        <aside>
          <div class="datos-usuario">
            <h2>Ingresa tus datos</h2>
            <div class="form">
              <label>Nombre *</label>
              <input v-model.trim="cliente.nombre" type="text" placeholder="Tu nombre">
              <label>E-mail</label>
              <input v-model.trim="cliente.email" type="email" placeholder="tucorreo@mail.com" />
              <small v-if="cliente.email && !emailValido" class="err">Email inválido</small>
            </div>
          </div>

          <div class="carrito">
            <h2>Resumen de Carrito</h2>
            <p>Subtotal <strong>${{ subtotal.toLocaleString('es-CL') }}</strong></p>
            <p>Descuento <strong>{{ descuento === 0 ? '0' :  '-$' + descuento.toLocaleString('es-CL') }}</strong></p>
            <h3>TOTAL <strong>${{ total.toLocaleString('es-CL') }}</strong></h3>
            <button>Siguiente</button>
          </div>
        </aside>
    </section>
    

</template>

<style lang="scss" scoped>
@use './PokeMartComp.scss';
</style>