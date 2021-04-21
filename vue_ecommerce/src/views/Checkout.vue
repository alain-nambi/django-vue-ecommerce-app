<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Listes des achats</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Produit</th>
                            <th>Prix</th>
                            <th>Quantité</th>
                            <th>TOTAL</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr 
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"    
                        >
                            <th>{{ item.product.name }}</th>
                            <th>${{ item.product.price }}</th>
                            <th>{{ item.quantity }}</th>
                            <th>${{ getItemTotal(item).toFixed(2) }}</th>

                        </tr>
                    </tbody>

                    <tfoot>
                        <tr>
                            <th></th>
                            <th>TOTAL</th>
                            <th>{{ cartTotalLength }}</th>
                            <th>${{ cartTotalPrice.toFixed(2) }}</th>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Détails de l'achat</h2>

                <p class="has-text-grey mb-4">* Tous les champs doivent être remplie</p>

                <div class="columns is-multiline">
                     <div class="column is-4">
                         <div class="field">
                             <label>First name</label>
                             <div class="control">
                                 <input type="text" class="input" v-model="first_name"> 
                             </div>
                         </div>
                     </div>

                    <div class="column is-4">
                        <div class="field">
                            <label>Last name</label>
                            <div class="control">
                                <input type="text" class="input" v-model="last_name">
                            </div>
                        </div>
                    </div>
                    <div class="column is-4">
                        <div class="field">
                            <label>E-mail</label>
                            <div class="control">
                                <input type="email" class="input" v-model="email">
                            </div>
                        </div>
                    </div>
                    <div class="column is-4">
                        <div class="field">
                            <label>Phone</label>
                            <div class="control">
                                <input type="text" class="input" v-model="phone">
                            </div>
                        </div>
                    </div>
                    <div class="column is-4">
                        <div class="field">
                            <label>Address*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address">
                            </div>
                        </div>
                    </div>
                    <div class="column is-4">
                        <div class="field">
                            <label>Zip code*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="zipcode">
                            </div>
                        </div>
                    </div>
                    <div class="column is-4">
                        <div class="field">
                            <label>Place*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="place">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="notification is-danger mt-4" v-if="errors.length">
                    <p v-for="error in errors" v-bind:key="error">{{ error }} </p>
                </div>

                <hr>

                <div id="card-element" class="mb-5">
                    <template v-if="cartTotalLength">
                        <hr>

                        <button class="button is-dark" @click="submitForm">Payer avec Ariary.net</button>
                    </template>
                </div>

            </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'Checkout',
    data() {
        return {
            cart: {
                items: []
            },
            stripe: {},
            card: {},
            first_name: '',
            last_name: '',
            email: '',
            phone: '',
            address: '',
            zipcode: '',
            place: '',
            errors: []
        }
    },
    mounted() {
        document.title = "E-COMMERCE | Checkout"

        this.cart = this.$store.state.cart
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        submitForm() {
            this.errors = []

            if(this.first_name == '' || this.last_name == '' || this.email == '' || this.phone == '' || this.address == '' || this.zipcode == '' || this.place == '') {
                toast({
                    message: "Remplisser tous les champs",
                    type: 'is-danger',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-left',
                })
            } else {       
                toast({
                    message: cartTotalPrice.toFixed(2) ,
                    type: 'is-success',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-right',
                })
            }
        }
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        }
    }
}
</script>