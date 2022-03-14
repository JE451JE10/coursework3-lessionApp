<template>
<div id="app">
    <header>
        <h1>{{sitename}}</h1>
        <button @click='showCheckout'>{{cartItemCount}}<span class="fas fa-shopping-cart"></span>Checkout</button>
    </header>
    <main>
        <div v-if="showCheckout">
            <div>
                <h1>Sort by</h1>
                <p>
                    <input type="radio" name="Attributes" @click="sortTable('Subject')">
                    <label for="Subject">Subject</label>
                    <input type="radio" name="Attributes" @click="sortTable('Location')">
                    <label for="Location">Location</label>
                    <input type="radio" name="Attributes" @click="sortTable('price')">
                    <label for="Price">Price</label>
                    <input type="radio" name="Attributes" @click="sortTable('Space')">
                    <label for="Space">Space</label>
                </p>
                <p>
                    <input type="radio" name="fav" @click="sortTable('asc')">
                    <label for="Subject">Ascending</label>
                    <input type="radio" name="fav" @click="sortTable('desc')">
                    <label for="Location">Descscending</label>
                </p>
            </div>
            <LessonList :lessons="lessons" @addlessons="addToCart" id="lesson"></LessonList>
        </div>
        <div else>
            <CheckoutList :cart="cart" @removelessons='deleteLesson' id="cart"></CheckoutList>
        </div>
    </main>
</div>
</template>

<script>
import LessonList from "./components/LessonList.vue";
import CheckoutList from "./components/CheckoutList.vue";
export default {
    name: "app",
    components: {
        LessonList,
        CheckoutList,
    },
    data() {
        return {
            sitename: "After School Lessons",
            cart: [],
            lessons: [],
        };
    },
    methods: {
        showCheckout() {
            this.showProduct = this.showProduct ? false : true;

        },
        addToCart(lessons) {
            this.cart.push(lessons)
            lessons.Space--; // the inventory can be easily updated   
        },
        deleteLesson(lessons) {
            if (this.cart.splice(this.cart.indexOf(lessons), 1)) {
                return lessons.Space++;
            }
        },
        sortTable(key, direction) {
            this.sort = `${key} > ${direction}`
            if (direction === 'asc') {
                this.lessons.sort((a, b) => a[key] > b[key] ? 1 : -1)
            } else {
                this.lessons.sort((a, b) => a[key] < b[key] ? 1 : -1)
            }
        }
    },
    created: function () {
        console.log("requesting data from server...");

        fetch('https://coursework2-lession.herokuapp.com/').then(res => res.text())
            .then(txt => console.log('server message: ', txt))
        fetch('https://coursework2-lession.herokuapp.com/collection/lesson')
            .then(res => res.json())
            .then(data => this.lessons = data)

    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 0px;
}

#lesson {
    display: flex;
        margin-top: 0px;
    flex-wrap: wrap;
    gap: 10px;
    border: 5px solid blue;
    background-color: lightblue
}

#cart {
   font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 0px;
    gap: 10px;
    border: 5px solid blue;
    background-color: lightblue
}
</style>
