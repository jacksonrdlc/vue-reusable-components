<template>
    <div>
        <select v-model="selectedOption" v-on:input="set">
            <option v-for="(option, name) in options" :value="option" :key="name">{{ name }}</option>
        </select>
    </div>
</template>

<script>
    export default {
        props: {
            value: null,
            options: {
                type: Object,
                required: true
            }
        },
        methods: {
            set() {
                this.$emit('input', event.target.value)
            }
        },
        data() {
            return {
                selectedOption: null
            }
        },
        mounted() {
            this.selectedOption = this.value
        },
        watch: {
            value: function(newValue) {
                this.selectedOption = newValue
            }
        }
    }
</script>