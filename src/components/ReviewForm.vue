<script setup>
import { reactive } from "vue";

const emit = defineEmits(["review-submitted"]);

const review = reactive({
    name: "",
    review: "",
    rating: null
});

const OnSubmit = () => {
    if (review.name === "" || review.content === "" || review.rating === null) {
        alert("Please fill out all fields");
        return;
    }

    const productReview = {
        name: review.name,
        content: review.content,
        rating: review.rating
    }
    emit("review-submitted", productReview);
    review.name = "";
    review.content = "";
    review.rating = null;
}

review.name = ""
review.content = ""
review.rating = null

</script>

<template>
    <form class="review-form" @submit.prevent="OnSubmit">
        <h3>Leave a review</h3>
        <label for="name">Name:</label>
        <input id = "name" v-model="review.name">

        <label for="review">Review:</label>
        <textarea id="review" v-model="review.content"></textarea>

        <label for="rating">Rating:</label>
        <select id="rating" v-model.number="review.rating">
            <option value="5">5</option>
            <option value="4">4</option>
            <option value="3">3</option>
            <option value="2">2</option>
            <option value="1">1</option>
        </select>

        <input class="button" type="submit" value="Submit">
    </form>
</template>