<script setup>
    import DrawerHead from './DrawerHead.vue';
    import CartItemList from './CartItemList.vue';
</script>



<template>
    <div class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-70 z-10"></div>
    <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8">
        <DrawerHead />
        <CartItemList />

        <div class="flex flex-col gap-4 mt-7">
            <div class="flex gap-2">
                <span>Итого:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>12990 ₽</b>
            </div>
            <div class="flex gap-2">
                <span>Налог 5%</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>990 ₽</b>
            </div>
            <button disabled="" class="mt-4 bg-lime-500 w-full rounded-xl disabled:bg-slate-300 py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer">
            Оформить заказ
        </button>
        </div>

        
    </div>
</template>