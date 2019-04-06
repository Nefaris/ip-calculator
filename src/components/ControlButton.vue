<template>
    <input :type="role" :value="text">
</template>

<script>
export default {
    name: 'ControlButton',
    props: {
        text: String,
        role: String
    }
}
</script>

<style scoped>
    input {
        width: 100%;
        padding: 15px;
        margin: 5px 0;
        font-size: 1.1em;
        border: none;
    }
</style>
