<template>
<div>
    <div v-for="lessons in lessons" :key="lessons.id">
        <figure>
            <img v-bind:src="lessons.image" width="100px">
        </figure>
        <p><b>Subject: {{lessons.Subject}}</b></p>
        <p><strong>Location: {{lessons.Location}}</strong></p>
        <p><b>Price: {{lessons.price}}</b></p>
        <p><b>Space: {{lessons.Space}}</b></p>
        <button @click="addToCart(lessons)"><span class="fas fa-cart-plus">
            </span>Add to Cart</button>
    </div>
</div>
</template>

<script>
export default {
    name: "LessonList",
    props: ['lessons'],
    data() {
        return {};
    },
    methods: {
        addToCart(lessons) {
            this.$emit('addlessons', lessons);
        },
    },
};
</script>
