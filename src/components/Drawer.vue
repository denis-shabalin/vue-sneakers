<script setup>
    import axios from 'axios';
    import { inject, ref, computed } from 'vue';

    import DrawerHead from './DrawerHead.vue';
    import CartItemList from './CartItemList.vue';
    import InfoBlock from './InfoBlock.vue';


    const props = defineProps({
        totalPrice: Number,
        vatPrice: Number
    })

    const { cart, closeDrawer } = inject('cart');

    const isCreating = ref(false);
    const orderId = ref(null);

    const createOrder = async () => {
    try {
        isCreating.value = true;
        
        const { data } = await axios.post(`https://cb0481a614ba4941.mokky.dev/orders`, {
            items: cart,
            totalPrice: props.totalPrice.value
        })

        cart.value = []

        orderId.value = data.id
        } catch (err) {
        console.log(err)
        } finally {
        isCreating.value = false;
        }
    }

    const cartIsEmpty = computed(() => cart.value.length === 0);  

    const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value);
</script>

<template>
    <div class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-70 z-10"></div>
    <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8">
        <DrawerHead />

        <div v-if="!totalPrice || orderId" class="flex h-full items-center">
            <InfoBlock
            v-if="!totalPrice && !orderId"
            title="Корзина пустая"
            description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
            image-url="/package-icon.png" />

            <InfoBlock
            v-if="orderId"
            title="Заказ оформлен"
            :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
            image-url="/order-success-icon.png" />
        </div>

        <div class="v-else">
            <CartItemList />
            <div class="flex flex-col gap-4 mt-7">
                <div class="flex gap-2">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ totalPrice }} ₽</b>
                </div>
                <div class="flex gap-2">
                    <span>Налог 5%</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ vatPrice }} ₽</b>
                </div>
                <button
                :disabled="buttonDisabled"
                @click="createOrder"
                class="mt-4 bg-lime-500 w-full rounded-xl disabled:bg-slate-300 py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer">
                Оформить заказ
            </button>
            </div>
        </div>

        
    </div>
</template>