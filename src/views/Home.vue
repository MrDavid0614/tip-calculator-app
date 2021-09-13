<template>
    <main class="main-container">
        <img src="../assets/images/logo.svg" alt="Logo">
        <Form />
    </main>
</template>

<script>
import Form from "../components/Form.vue"

export default {
    components: {
        Form,
    }
}
</script>

<style scoped>
.main-container {
    display: flex;
    justify-content: center;
    padding: 50px 0;
    gap: 50px;
}
</style>