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
 
</template>

<style scoped>

</style>