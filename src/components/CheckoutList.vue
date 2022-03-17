<template>
<div>
    <h2>Checkout</h2>
    <div id="lesson"> 
    <div v-for="lessons in cart" :key="lessons.id">
        <figure>
            <img v-bind:src="lessons.image" width="100px">
        </figure>
        <p><b>Subject: {{lessons.Subject}}</b></p>
        <p><strong>Location: {{lessons.Location}}</strong></p>
        <p><b>Price: {{lessons.price}}</b></p>
        <p><b>Space: {{lessons.Space}}</b></p>
        <button @click='deleteLesson(lessons)'><span class="fas fa-cart-arrow-down"></span>Delete</button>
    </div>
    </div>
    <div id="app">
        <button @click="resetInput">Reset Input Checkout</button>
        <p>
            <strong>First Name:</strong>
            <input v-model="order.FirstName" title="Letters only." required />
        </p>
        <p>
            <strong>Second Name:</strong>
            <input v-model="order.SecondName" title="Letters only." required />
        </p>
        <p>
            <strong>Phone Number:</strong>
            <input v-model="order.Phonenumber" minlength="10" title="Numbers only." required />
        </p>
        <h2>Order Information</h2>
        <p>Name: {{order.FirstName}}</p>
        <p>Name: {{order.SecondName}}</p>
        <p>Phone number: {{order.Phonenumber}}</p>
        <button :disabled='!valid' @click="submit" type="submit">Checkout</button>
    </div>
</div>
</template>

<script>
export default {
    name: "CheckoutList",
    props: ['cart'],
    data() {
        return {
            order: {
                FirstName: '',
                SecondName: '',
                Phonenumber: '',
            }
        };
    },
    methods: {
        deleteLesson(lessons) {
            this.$emit('removelessons', lessons)
        },
        submit() {
            var AddOrder = {
                order: this.cart,
                FirstName: this.order.FirstName,
                SecondName: this.order.SecondName,
                Phonenumber: this.order.Phonenumber,
            };
            // set the url to your server and route
            fetch("https://coursework2-lession.herokuapp.com/collection/order", {
                    method: "POST", // set the HTTP method as 'POST'
                    headers: {
                        "Content-Type": "application/json", // set the data type as JSON
                    },
                    body: JSON.stringify(AddOrder), // need to stringify the JSON object
                })
                .then(response => response.json())
                .then(responseJSON => {
                    console.log('Success: Add ', responseJSON);
                });

            if (this.cart.length > 0) {
                window.alert("Order Submited")
            }
        },
        resetInput() {
            this.order.FirstName = "";
            this.order.SecondName = "";
            this.order.Phonenumber = "";
        },
    },
    //82:14  error  Expected to return a value in "valid" computed property  vue/return-in-computed-property//
    computed: {
        valid() {
            var TxtValid = /^[a-zA-Z]+$/
            var IntValid = /^\d+$/
            if (TxtValid.test(this.order.FirstName) && TxtValid.test(this.order.SecondName) &&
                IntValid.test(this.order.Phonenumber) && (this.cart.length > 0)) {
                return true;
            } else {
                return false
            }
        },
    },

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
    flex-wrap: wrap;
    gap: 10px;
    border: 5px solid blue;
    background-color: lightblue
}


</style>
