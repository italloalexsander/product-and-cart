<template>
    <aside class="cart-container">
      <div class="cart">
        <h1 class="cart-title spread">
          <span>
            Cart
            <i class="icofont-cart-alt icofont-1x"></i>
          </span>
          <button @click="toggle" class="cart-close">&times;</button>
        </h1>

        <div class="cart-body">
          <table class="cart-table">
            <thead>
              <tr>
                <th><span class="sr-only">Proreturn ((this.cart.carrots * 4.82) + (this.cart.pineapples * 1.62) + (this.cart.cherries * 1.04)).toFixed(2)duct Image</span></th>
                <th>Product</th>
                <th>Price</th>
                <th>Qty</th>
                <th>Total</th>
                <th><span class="sr-only">Actions</span></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(quantity, key, i) in cart" :key="i">
                <td><i :class="'icofont-' + getIcon(key) + ' icofont-3x'"></i></td>
                <td>{{ key }}</td>
                <td>${{getPrice(key).toFixed(2)}}</td>
                <td class="center">{{ quantity }}</td>
                <td>${{(quantity * getPrice(key)).toFixed(2) }}</td>
                <td class="center">
                  <button @click="remove(key)" class="btn btn-light cart-remove">
                    &times;
                  </button>
                </td>
              </tr>
            </tbody>
          </table>

          <p v-if="!Object.keys(cart).length" class="center"><em>No items in cart</em></p>
          <div class="spread">
            <span><strong>Total:</strong> ${{ (cartTotal()).toFixed(2) }} </span>
            <button class="btn btn-light">Checkout</button>
          </div>
        </div>
      </div>
    </aside>
</template>

<script>
export default {
  props: ['toggle', 'cart', 'inventory', 'remove'],
  methods: {
    getPrice (name) {
      const productName = this.inventory.find((product) => {
        return product.name === name
      })

      return productName.price.USD
    },
    getIcon (name) {
      const productName = this.inventory.find((product) => {
        return product.name === name
      })
      console.log(productName.icon)
      return productName.icon
    },
    cartTotal () {
      const total = Object.entries(this.cart).reduce((sum, currentItem, index) => {
        return sum + (currentItem[1] * this.getPrice(currentItem[0]))
      }, 0)

      return total
    }
  }
}
</script>
