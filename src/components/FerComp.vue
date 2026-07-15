<script setup></script>

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
            <div class="producto">
                <p>Full Restore <strong>$5000</strong></p>
                <button>+ Agregar</button>
            </div>

            <h2>Carrito</h2>
            <p>El carrito está vacío, entrenador. ¡Busca algún producto!</p>
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