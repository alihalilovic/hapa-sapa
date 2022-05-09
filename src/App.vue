<script setup>
  import {onMounted, ref, watch } from "vue"
  
  import dogUrl from "./assets/dog.png"
  import catUrl from "./assets/cat.png"

  const foodType = ref("pse")
  const imageUrl = ref(dogUrl)
  const chosenVariant = ref("Mala")
  const price = ref(0.0)
  const quantity = ref(1)

  const orderModalVisibility = ref(false)

  const productOptions = ref([
    {
      label: "Pas",
      type: "pse",
      src: dogUrl,
      alt: "Slika psa"
    },
    {
      label: "Mačka",
      type: "mačke",
      src: catUrl,
      alt: "Slika mačke"
    }
  ])

  const variants = [
    { id: 1, title: "Mala", abbr: "S", price: 2.5 },
    { id: 2, title: "Srednja", abbr: "M", price: 5},
    { id: 3, title: "Velika", abbr: "L", price: 10},
  ]

  const calculatePrice = () => {
    const variant = variants.filter(v => v.title == chosenVariant.value)[0]
    price.value = variant.price * quantity.value
  }

  const handleVariantClick = (title) => {
    chosenVariant.value = title
    calculatePrice()
  }

  const handleOrderSubmit = () => {
    orderModalVisibility.value = false
    window.alert("Proizvod narucen")
  }


  watch(foodType, () => {
    productOptions.value.forEach(product => {
      if (product.type == foodType.value) {
        imageUrl.value = product.src
        return
      }
    })
  })


  onMounted(() => {
    calculatePrice()
  })
</script>

<template>
<nav>
  <img src="./assets/logo.jpg" alt="Logo">
</nav>
<div class="container">
  <section class="product-image-container">
    <img :src="imageUrl">
  </section>
  <section class="product-options">
    <div class="food-type">
      <h1>Hrana za {{ foodType }}</h1>
      <button
        v-for="option in productOptions"
        @click="foodType = option.type"
        class="btn"
        >
        {{ option.label }}
      </button>
    </div>

    <div class="food-variant">
      <h2>Veličina porcije</h2>
      <button
        class="btn variant"
        v-for="variant in variants"
        @click="handleVariantClick(variant.title)"
      >
        {{ variant.abbr }}
      </button>
      <p>
        Odabrana porcija: {{ chosenVariant }}
      </p>
    </div>

    <div class="food-quantity">
      <h1>Količina</h1>
      <input v-model="quantity" type="number" @change="calculatePrice">
    </div>

    <div class="food-order">
      <button @click="orderModalVisibility = !orderModalVisibility" class="btn order">Naruči</button>
    </div>

    <p>Cijena - {{ price }}KM</p>
  </section>

  <div class="modal" v-if="orderModalVisibility">
    <form @submit.prevent="handleOrderSubmit">
      <label>Ime i prezime:
        <input type="text" />
      </label>
      <label>Broj telefona:
        <input type="text" />
      </label>
      <label>Adresa:
        <input type="text" />
      </label>
      <label>Postanski broj:
        <input type="text" />
      </label>
      <button class="btn order order-modal">Naruci</button>
    </form>
  </div>
</div>

</template>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

* {
  font-family: "Roboto";
}

nav > img {
  width: 100px;
  border-radius: 20px;
  filter:saturate(0);
}

nav > img:hover {
  filter:saturate(1);
  cursor: pointer;
}

nav {
  display: flex;
  padding: 1rem;
}

.container {
  display: flex;
  justify-content: center;
  width: 70%;
  margin: auto;
}

.container > section {
  margin: 1rem;
}

.product-options {
  width: 50%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
}

.product-image-container {
  background: #eee;
  border-radius: 20px;
}

.product-image-container img {
  width: 600px;
  height: 500px;
  object-fit: contain;
  padding: 2rem;
}

.food-order {
  width: 100%;
}

.food-order > * {
  flex: 2;
}

.btn {
  border-radius: 30px;
  outline: none;
  border: none;
  background: lightseagreen;
  padding: .5rem 1.5rem;
  text-transform: uppercase;
  font-weight: bold;
  color: white;
  margin-right: .5rem;
  transition: 0.2s ease;
}

.btn:hover {
  opacity: 0.9;
  cursor: pointer;
  transform: scale(1.1);
}

.variant {
  padding: 25px 30px;
  border-radius: 50%;
}

.order {
  width: 100%;
  padding: 1.2rem 3rem;
  border-radius: 10px;
}

.modal {
  background: rgba(0, 0, 0, 0.5);
  height: 100vh;
  width: 100vw;
  z-index:999;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal form {
  display: flex;
  flex-direction: column;
  width: 50%;
  background: #ccc;
  border-radius: 20px;
  padding: 2rem;
  margin: auto;
}

.modal form label {
  padding: 2rem;
  display: flex;
  flex-direction: column;
}

.modal form input {
  padding: .5rem;
}

.order-modal {
  padding: 1rem;
  width: 50%;
  margin: auto;
}
</style>