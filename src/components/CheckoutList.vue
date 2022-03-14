<template>
<div>
    <h2>Checkout</h2>
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
    <button @click="resetInput">Reset Input Checkout</button>
    <p>
        <strong>First Name:</strong>
        <input v-model="FirstName" title="Letters only." required />
    </p>
    <p>
        <strong>Second Name:</strong>
        <input v-model="SecondName" title="Letters only." required />
    </p>
    <p>
        <strong>Phone Number:</strong>
        <input v-model="Phonenumber" minlength="10" title="Numbers only." required />
    </p>
    <h2>Order Information</h2>
    <p>Name: {{FirstName}}</p>
    <p>Name: {{SecondName}}</p>
    <p>Phone number: {{Phonenumber}}</p>
    <button :disabled='!valid' @click="submit" type="submit">Checkout</button>
</div>
</template>

<script>
export default {
    name: "CheckoutList",
    props: ['cart'],
    data() {
        return {
            FirstName: '',
            SecondName: '',
            Phonenumber: '',
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
                SpaceOrdered: this.lessons.Space
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
            this.FirstName = "";
            this.SecondName = "";
            this.Phonenumber = "";
        },
    },
    //82:14  error  Expected to return a value in "valid" computed property  vue/return-in-computed-property//
    computed: {
        valid() {
            var TxtValid = /^[a-zA-Z]+$/
            var IntValid = /^\d+$/
            if (TxtValid.test(this.FirstName) && TxtValid.test(this.SecondName) &&
                IntValid.test(this.Phonenumber) && (this.cart.length > 0)) {
                return true;
            }

        },
    },
};
</script>
