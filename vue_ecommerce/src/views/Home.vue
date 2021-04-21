<template>

  <div class="home">
    <section class="hero is-medium is-success mb-6">
      <div class="hero-body has-text-centered" >
        <p class="title mb-6">
          Bienvenue sur le site E-SPORT
        </p>
        <p class="subtitle">
          La meilleure site en ligne où vous trouverez toutes sortent d'articles de sport notamment le football
        </p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Nouvelles arrivages</h2>
      </div>

      <ProductBox 
        v-for="product in latestProducts"
        v-bind:key="product.id"
        v-bind:product="product" />
    </div>

  </div>

</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
 mounted() {
    this.getLatestProducts()

    document.title = 'E-SPORT | Accueil'
  },
  methods: {
  async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error =>{
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

