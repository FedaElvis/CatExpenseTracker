<template>
    <h2>Your Balance</h2>
    <h1 id="balance">{{ props.total }}</h1>
</template>

<script setup>
const props = defineProps({total:{
    type:Number,
    required: true
}})
</script>

<style scoped>
h2{
    color: green;
}
</style>