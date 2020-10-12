<template>
    <li class="review-preview">
        <h3>Name: {{review.name}}</h3>
        <h3>Rate: {{review.rate}}</h3>
        <h3>Read At: {{review.readAt}}</h3>
        <h3>Review: {{review.freeTxt}}</h3>
    </li>
</template>

<script>
export default {
    props: ['review']
}
</script>