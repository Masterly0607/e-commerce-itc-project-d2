<template>
  <section class="f-container-1200">
    <div class="mt-3">
      <div class="flex items-center gap-3">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-10 text-black font-bold">
          <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 0 0-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 0 0-16.536-1.84M7.5 14.25 5.106 5.272M6 20.25a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0Zm12.75 0a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0Z" />
        </svg>
        <div class="text-3xl font-bold">Cart</div>
      </div>
      <div class="ml-14 text-gray-400 font-thin">All items to checkout</div>
    </div>

    <div class="p-8 flex flex-col lg:flex-row gap-8">
      <div class="flex-1 bg-white p-12 shadow-sm rounded-lg lg:flex-row">
        <h2 class="text-xl font-bold">Shopping Bag</h2>
        <p class="text-gray-600 mb-6">{{ cartItems.length }} items in your bag</p>

        <div v-for="(item, index) in cartItems" :key="index" class="flex items-center justify-between mb-4">
          <div class="flex items-center">
            <img :src="item.image" alt="Product Image" class="w-16 h-16 rounded-lg mr-4" />
            <div>
              <h3 class="text-gray-800">{{ item.name }}</h3>
              <p class="text-sm text-gray-500">${{ item.price }}</p>
            </div>
          </div>
          <div class="flex items-center">
            <select v-model="item.quantity" class="border rounded-md px-2 py-1 text-gray-700 mr-4">
              <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
            </select>
            <p class="text-gray-800">${{ item.price * item.quantity }}</p>
            <button @click="removeItem(item)" class="ml-4 text-red-500">Remove</button>
          </div>
        </div>
      </div>

      <div class="w-full lg:w-96 bg-white shadow-md p-6 rounded-lg">
        <h3 class="text-lg font-bold mb-4">Calculated Shipping</h3>
        <div class="space-y-4 mb-6">
          <div>
            <label class="text-sm text-gray-600">Country</label>
            <select class="w-full border rounded-md px-3 py-2 text-gray-700">
              <option value="cambodia">Cambodia</option>
            </select>
          </div>
          <div>
            <label class="text-sm text-gray-600">State/City</label>
            <select class="w-full border rounded-md px-3 py-2 text-gray-700 ">
              <option>Phnom Penh</option>
              <option>Batdambang</option>
              <option>Kandal</option>
              <option>Prey Veng</option>
              <option>Takeo</option>
              <option>Siem Reap</option>
              <option>Svay Rieng</option>
              <option>Ratanakiri</option>
              <option>Kampong Cham</option>
              <option>Kampong Chhnang</option>
              <option>Kampong Speu</option>
              <option>Kampong Thom</option>
              <option>Kratie</option>
              <option>Oddar Meanchey</option>
              <option>Pailin</option>
              <option>Preah Vihear</option>
              <option>Kep</option>
              <option>Tbong Khmum</option>
              <option>Sihanoukville</option>
              <option>Mondulkiri</option>
              <option>Banteay Meanchey</option>
              <option>Koh Kong</option>
            </select>
          </div>
          <div>
            <label class="text-sm text-gray-600">ZIP Code</label>
            <input type="text" v-model="zipCode" @input="validateZipCode" :class="{'error': zipCodeError}"
            class="w-full border rounded-md px-3 py-2 text-gray-700" />
            <span class="message-error" v-if="zipCodeError">{{ zipCodeError }}</span>
          </div>
          <button @click="updateZipCode" :disabled="!isZipCodeValid || !zipCodeChanged"
          class="w-full bg-black rounded-md px-4 py-2 text-white hover:bg-gray-500">Update</button>
        </div>

        <div class="mb-6">
          <h4 class="text-sm font-semibold mb-2">Coupon Code</h4>
          <p class="text-xs text-gray-500 mb-2">Add a coupon if you have one!</p>
          <div class="flex gap-2">
            <input type="text" placeholder="Coupon code..." v-model="couponCode" :class="{'error': couponError}"
            class="flex-1 border rounded-md px-3 py-2 text-gray-700" />
            <button @click="applyCoupon" :disabled="!couponCode"
            class="bg-black rounded-md px-4 py-2 text-white hover:bg-gray-500">
            {{ couponLoading ? 'Apply...' : 'Apply' }}
          </button>
          </div>
          <span v-if="couponError" class="text-red-500 text-sm mt-1">{{ couponError }}</span>
          <span v-if="couponSuccess" class="text-green-500 text-sm mt-1">{{ couponSuccess }}</span>
        </div>

        <div class="bg-black p-4 rounded-md text-white hover:bg-gray-500">
          <p class="flex justify-between">
            <span>Cart Subtotal</span>
            <span>${{ cartSubtotal.toFixed(2) }}</span>
          </p>

          <p v-if="automaticDiscount > 0" class="flex justify-between mb-2 text-red-500">
            <span>Amount discount ({{ discountAsPercentage }}%)</span>
            <span>-${{ automaticDiscount.toFixed(2) }}</span>
          </p>

          <p class="flex justify-between font-bold">
            <span>Cart Total</span>
            <span>${{ finalTotal.toFixed(2) }}</span>
          </p>
        </div>
      <router-link :to="{name: 'checkout'}">
        <button class="mt-4 w-full bg-black text-white rounded-md px-4 py-2 hover:bg-gray-500 cursor-pointer" @click="proceedToCheckout()">
          Proceed to Checkout
        </button>
      </router-link>
      </div>
    </div>
  </section>
</template>

<script>
import {useProductStore} from '@/stores/productStore';

export default {
  setup() {
    const productStore = useProductStore()
    return{
      productStore,
    }
  },
  data() {
    return {
      couponRate: 0,

      zipCode: '',
      originalZipCode: '',
      zipCodeError: '',
      isZipCodeValid: false,
      zipCodeChanged: false,
      couponCode: '',
      couponError: '',
      couponSuccess: '',
      couponLoading: false,

      appliedDiscount: 0,
      shippingCost: 0
    }
  },
  computed: {
    cartItems() {
      const cart = useProductStore().state.cart;
      console.log('Cart Items:', cart);
      return cart;
    },
    cartSubtotal() {
      return this.cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
    },
    discountAsPercentage() {
        if (this.cartSubtotal >= 200) return 3;
        if (this.cartSubtotal >= 100) return 2;
      },

      automaticDiscount() { 
        return (this.cartSubtotal >= 100) ? (this.cartSubtotal * 0.02) : 0;
      },

      couponDiscount() {
        return (this.cartSubtotal * (this.appliedDiscount / 100)) || 0;
      },

      finalTotal() {
        const subtotal = this.cartSubtotal || 0;
        if (subtotal === 0) {
          return 0;
        }

        let discount = this.automaticDiscount + this.couponDiscount;
        return subtotal - discount;
      }
  },

  methods: {
    removeItem(item) {
      const productStore = useProductStore();
      productStore.removeFromCart(item.name);
    },

    validateZipCode() {
      this.zipCodeChanged = this.zipCode !== this.originalZipCode
      const zipRegex = /^\d{5}$/;

      if (!this.zipCode) {
        this.zipCodeError = 'Zip code is required'
        this.isZipCodeValid = false
      }else if(!zipRegex.test(this.zipCode)) {
        this.isZipCodeError = 'Please enter a valid 5-digit ZIP code'
        this.isZipCodeValid = false
      }else {
        this.zipCodeError = ''
        this.isZipCodeValid = true
      }
    },

    async updateZipCode() {
      try {
        this.validateZipCode();
        if (!this.isZipCodeValid) {
          return;
        }

        const response = await this.simulateApiCall('updateZip', this.zipCode);

    if (response.success) {
      // Update the original ZIP code if the API call is successful
      this.originalZipCode = this.zipCode;
      this.zipCodeChanged = false;
      this.zipCodeError = ''; // Clear the error if successful

      // Recalculate the shipping based on the updated ZIP code
      this.calculateShipping();
    }
  } catch (error) {
    this.zipCodeError = 'Failed to update ZIP code. Please try again.';
    console.error('Error updating ZIP code:', error);
  }
    },
    

    async applyCoupon() {
      if (!this.couponCode) return
      this.couponLoading = true
      this.couponError = ''
      this.couponSuccess = ''

      try {
        const response = await this.simulateApiCall('applyCoupon', this.couponCode)
        if (response.success) {
          this.appliedDiscount = response.discount
          this.couponSuccess = `Coupon "${this.couponCode}" applied successfully! (${response.discount}% off)`

          this.$forceUpdate();
        }
      // eslint-disable-next-line no-unused-vars
      } catch (error) {
        this.couponError = 'Invalid coupon code. Please try again.'
        this.appliedDiscount = 0
      } finally {
        this.couponLoading = false
      }
    },

    calculateShipping() {
      if(this.isZipCodeValid) {
        this.shippingCost = 5.99
      }
      // this.shippingCost = 5.99
    },

    proceedToCheckout() {
      const checkoutData = {
        items: this.cartItems,
        subtotal: this.cartSubtotal,
        discount: this.automaticDiscount,
        shipping: this.shippingCost,
        total: this.finalTotal,
        zipCode: this.zipCode
      }
      console.log("Checkout Data:", checkoutData); // Debugging line

      localStorage.setItem('checkoutData', JSON.stringify(checkoutData)); // Store in local storage

      this.$router.push({ name: 'checkout' }); // Navigate to checkout
    },

    simulateApiCall(type, value) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (type === 'updateZip') {
            resolve({ success: true })
          } else if (type === 'applyCoupon') {
            if (value.toUpperCase() === 'FIG123') {
              resolve({ success: true, discount: 3 })
            } else {
              reject(new Error('Invalid coupon'))
            }
          }
        }, 1000)
      })
    }
  },
};
</script>
