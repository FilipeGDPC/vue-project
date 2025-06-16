<template>
    <form>
        <h3>Review Form</h3>
        <label for="name">Name:</label>
        <input id = "name">

        <label for="review">Review:</label>
        <textarea id="review"></textarea>

        <label for="rating">Rating:</label>
        <select id="rating">
            <option value="5">5</option>
            <option value="4">4</option>
            <option value="3">3</option>
            <option value="2">2</option>
            <option value="1">1</option>
        </select>

        <input class="button" type="submit" value="Submit">
    </form>
</template>