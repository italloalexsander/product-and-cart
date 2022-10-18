<template>
  <header class="top-bar spread">
        <nav class="top-bar-nav">
          <router-link to="/" class="top-bar-link">
            <i class="icofont-spoon-and-fork"></i>
            <span>Home</span>
          </router-link>
          <router-link to="/products" class="top-bar-link">
            <span>Products</span>
          </router-link>
          <router-link to="past-orders" class="top-bar-link">
            <span>Past Orders</span>
          </router-link>
        </nav>
        <div @click="toggleSidebar" class="top-bar-cart-link">
          <i class="icofont-cart-alt icofont-1x"></i>
          <span>Cart ({{ totalQuantity }})</span>
        </div>
      </header>
  <router-view :inventory="inventory"/>
  <SideBar
    v-if="showSideBar"
    :toggle="toggleSidebar"
    :cart="cart"
    :inventory="inventory"
    :remove="removeItem"
  />
</template>

<script>
import SideBar from '@/components/SideBar.vue'
import food from './food.json'

export default {
  components: {
    SideBar
  },
  data () {
    return {
      showSideBar: true,
      inventory: food,
      cart: {}
    }
  },
  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((acc, curr) => {
        return acc + curr
      }, 0)
    }
  },
  methods: {
    addToCart (name, index) {
      if (!this.cart[name]) this.cart[name] = 0
      this.cart[name] += this.inventory[index].quantity
      this.inventory[index].quantity = 0
    },
    toggleSidebar () {
      this.showSideBar = !this.showSideBar
    },
    removeItem (key) {
      delete this.cart[key]
    }
  },
  async mounted () {
    const res = await fetch('./food.json')
    const data = await res.json()
    this.inventory = data
  }
}
</script>
