<template>
    <section v-if="books" class="book-app">
        <BookList :books="booksToShow"/>
    </section>
</template>

<script>
import { bookService } from "@/services/bookService.js";
import BookList from "@/cmps/BookList.vue";

export default {
    data(){
        return {
            books: null,
            filterBy: {
                title: ''
            }
        }
    },
    async created(){
        const books = await bookService.query()
        this.books = books
    },
    computed: {
        booksToShow(){
            const filterForRegEx = (this.filterBy.title.length) ? `${this.filterBy.title}*`:''
            const filterRegEx = new RegExp(filterForRegEx,'i')
            return this.books.filter(book => {return filterRegEx.test(book.title)})
        }
    },
    components: {
        BookList,
    }
}
</script>