<script>
export default {
    props: ["data", "highlighted"],
    created() {
        console.log(this.data);
    },
    methods: {
        elementClass(id) {
            if(id === this.highlighted?.id) return "highlighted"
        }
    }
}
</script>
<template>
    <h2>{{ data.name }}</h2>
    <ul>
        <li v-for="child in data.children" :class="elementClass(child.id)" @click="$emit('click', child.id)">
            {{ child.name }}
        </li>
    </ul>
</template>
<style scoped>
.highlighted {
    color: red;
}
</style>