<script setup>
import { ref, defineEmits } from "vue";
const text = ref("");
const emits = defineEmits(["onNewItem"]);

function handleSubmit() {
    if (text.value != '') {
        //manda a llamar el emit
        emits("onNewItem", text);
        text.value = "";
    }
}
</script>

<template>
    <form action="" @submit.prevent="handleSubmit">
        <input type="text" v-model="text">
    </form>
</template>

<style scoped>

input{
    width: 100%;
    box-sizing: border-box;
}
form {
    margin-bottom: 10px;
}

</style>