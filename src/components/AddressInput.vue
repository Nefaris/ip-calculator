<template>
    <input v-bind:value="value" v-bind:maxlength="maxlen" @input="handleChange">
</template>

<script>
export default {
    name: 'AdressInput',
    props: {
        value: String,
        maxlen: String
    },
    methods: {
        handleChange(e) {
            this.$emit('input', e.target.value);
        }
    }
}
</script>

<style scoped>
    input {
        height: 40px;
        max-width: 55px;
        border: none;
        border-bottom: solid 1px #000;
        text-align: center;
        font-size: 1.5em;
    }

    input:focus {
        outline: none;
    }
</style>
