<template>
    <tr>
        <th><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></th>
        <th>{{ item.product.price }} $ </th>
        <th>
            <button class="mr-2" @click="decrementQuantity(item)">-</button>
            {{ item.quantity }}
            <button class="ml-2" @click="incrementQuantity(item)">+</button>
        </th>
        <th>${{ getItemTotal(item).toFixed(2) }}</th>
        <th><button class="delete" @click="removeFromCart(item)"></button></th>
    </tr>
</template>

<script>
export default {
    name: 'CartItem',
    props: {
        initialItem: Object
    },
    data() {
        return {
            item: this.initialItem
        }
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        decrementQuantity(item) {
            item.quantity -= 1

            if(item.quantity === 0) {
                this.$emit('removeFromCart', item)
            }

            this.updateCart()
        },
        incrementQuantity(item) {
            item.quantity += 1

            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item)

            this.updateCart()
        },
    },
}
</script>