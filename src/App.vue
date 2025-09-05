<template>
<div class="container basket">
      <table class="basket-table">
        <thead class="basket-table__header">
          <tr>
            <th>Product Details</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Subtotal</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody class="basket-table__body">
          <tr v-for="(product, index) in basket" :key="product.id">
            <td>
              <div class="basket-item">
                <div class="basket-item__image">
                  <img :src="product.imageUrl" alt="" />
                </div>
                <div class="basket-item__info">
                  <h2 class="basket-item__info-h2">{{ product.name }}</h2>
                  <p class="basket-item__info-p">Color: {{ product.color }}</p>
                  <p class="basket-item__info-p">Size: {{ product.size }}</p>
                </div>
              </div>
            </td>
            <td>
              <p class="basket-item__price">$ {{ product.price }}</p>
            </td>
            <td>
              <div class="basket-item__quantity">
                <button class="quantity-button" @click="decreaseItemQuantity(product.name)">–</button>
                <input type="number" :value="product.quantity" min="1" />
                <button class="quantity-button" @click="increaseItemQuantity(product.name)">+</button>
              </div>
            </td>
            <td>
              <p class="basket-item__price">$ {{ subtotal(product) }}</p>
            </td>
            <td>
              <button class="btn btn-delete" aria-label="Удалить" @click="removeItem(index)">
                <svg
                  class="w-6 h-6 text-gray-800 dark:text-white"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 7h14m-9 3v8m4-8v8M10 3h4a1 1 0 0 1 1 1v3H9V4a1 1 0 0 1 1-1ZM6 7h12v13a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1V7Z"
                  />
                </svg>
              </button>
            </td>
          </tr>

          <tr v-if="!basket.length">
            <td colspan="5">
              <p class="basket-table__empty">No items</p>
            </td>
          </tr>

          <tr v-else>
            <td colspan="5">
              <div class="basket-table__summary">
                <p class="basket-table__total">Total <b>$ {{ totalPrice.toFixed(2) }}</b></p>
                <p>Tax $ {{ totalTax }}</p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>
<script setup>
  import { computed, reactive } from 'vue';
  const basket = reactive([]);
  const defaultBasket = [
    {
      id: 1,
      name: 'Blue Flower Print Crop Top',
      color: 'Yellow',
      size: 'M',
      price: 29.0,
      quantity: 1,
      imageUrl: 'src/assets/crop-top.png',
    },
    {
      id: 2,
      name: 'Levender Hoodie',
      color: 'Levender',
      size: 'XXL',
      price: 119.00,
      quantity: 1,
      imageUrl: 'src/assets/hoodie.png',
    },
    {
      id: 3,
      name: 'Black Sweatshirt',
      color: 'Black',
      size: 'XXL',
      price: 123.00,
      quantity: 1,
      imageUrl: 'src/assets/sweatshirt.png',
    },
  ]

  let localBasket = localStorage.getItem("totalBasket");
  if (localBasket) {
    basket.push(...JSON.parse(localBasket))
  } else {
    basket.push(...defaultBasket)
  }
  function increaseItemQuantity(item) {
    basket.forEach(product => {
      if (product.name === item) {
        product.quantity++
        localStorage.setItem("totalBasket", JSON.stringify(basket))
      }

    })
  }

  function decreaseItemQuantity(item) {
    basket.forEach(product => {
      if (product.quantity < 2) return

      if (product.name === item) {
        product.quantity--
        localStorage.setItem("totalBasket", JSON.stringify(basket))
      }

    })
  }

  function removeItem(index) {
    basket.splice(index, 1)
    localStorage.setItem("totalBasket", JSON.stringify(basket))
    if (!basket.length) {
      localStorage.removeItem("totalBasket")
    }
  }

  const subtotal = computed(()=> {
    return (product) => product.price * product.quantity
  })

  const totalPrice = computed(()=> {
    const totalPrice = basket.reduce((acc, product) => {
      return (acc + product.price * product.quantity)
    }, 0)
    return totalPrice
  })

  const totalTax = computed(() => {
    return (totalPrice.value * 0.1).toFixed(2)
  })

</script>
<style src="./App.css"></style>
