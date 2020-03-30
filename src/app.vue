<template>
  <div id="app">
    <searchString v-model="searchText"></searchString>
    <queryForm v-model="queryObject"></queryForm>
    <input type="button" @click="showNewBookForm()" value=" Додати кнгу" />
    <bookTable v-bind:bookList="filtredBooks" @remove="removeBook" @update="showUpdateBookForm"></bookTable>

    <bookForm v-model="book" v-bind:visible="formVisible" @input="formInput"></bookForm>
    
  </div>
</template>

<script>
import Vue from "vue";
import bookTable from "./components/bookTable";
import bookForm from "./components/bookForm";
import searchString from "./components/searchString";
import queryForm from "./components/queryForm"

export default {
  name: "app",
  data() {
    return {
      books: [
        {
          authors: [
            "Erich Gamma",
            "Richard Helm",
            "Ralph Johnson",
            "John Vlissides"
          ],
          _id: "5e80e35446ce860b8ca62027",
          title:
            "Design Patterns: Elements of Reusable Object-Oriented Software",
          isbn: "0-201-63361-2",
          published: "1997-01-10T22:00:00.000Z",
          pages: 368,
          price: 449.95,
          __v: 0
        },
        {
          authors: ["Donald E. Knuth"],
          _id: "5e80e35446ce860b8ca62028",
          title: "The Art of Computer Programming. Vol.1",
          isbn: "0-201-89683-4",
          published: "1974-01-31T21:00:00.000Z",
          pages: 735,
          price: 799,
          __v: 0
        },
        {
          authors: ["Donald E. Knuth"],
          _id: "5e80e35446ce860b8ca62029",
          title: "The Art of Computer Programming. Vol.2",
          isbn: "0-201-89683-4",
          published: "1974-01-31T21:00:00.000Z",
          pages: 735,
          price: 799,
          __v: 0
        },
        {
          authors: ["Donald E. Knuth"],
          _id: "5e80e35446ce860b8ca6202a",
          title: "The Art of Computer Programming. Vol.3",
          isbn: "0-201-89683-4",
          published: "1974-01-31T21:00:00.000Z",
          pages: 735,
          price: 799,
          __v: 0
        },
        {
          authors: ["Марно Каскиаро", "Лучано Маммино"],
          _id: "5e80e35446ce860b8ca6202b",
          title: "Шаблоны проектирования Node.JS",
          isbn: "978-5-97060-485-4",
          published: "2017-05-03T21:00:00.000Z",
          pages: 396,
          price: 960,
          __v: 0
        },
        {
          authors: ["Дейли Брэд"],
          _id: "5e80e35446ce860b8ca6202c",
          title:
            "Разработка веб-приложений с помощью Node.js, MongoDB и Angular. Исчерпывающее руководство по использованию стека MEAN",
          isbn: "978-5-6040044-8-7",
          published: "2017-08-31T21:00:00.000Z",
          pages: 656,
          price: 588,
          __v: 0
        },
        {
          authors: [
            "Erich Gamma",
            "Richard Helm",
            "Ralph Johnson",
            "John Vlissides"
          ],
          _id: "5e80e57bb25b5235f4a8d690",
          title: "GOF",
          isbn: "0-201-63361-2",
          published: "1997-01-10T22:00:00.000Z",
          pages: 368,
          price: 449.95,
          __v: 0
        }
      ],
      book: {
        title: "",
        authors: [],
        isbn: "",
        published: "1997-01-10T22:00:00.000Z",
        pages: 0,
        price: 0
      },
      formVisible: false,
      selectedIndex: -1,
      searchText: "",
      queryObject:{
          minPages: null,
          maxPages: null,
          maxPrice: null
      }
    };
  },
  components: {
    bookTable,
    bookForm,
    searchString,
    queryForm
  },
  computed: {
    filtredBooks() {
     
      let result =  this.books;
      if (this.searchText) 
        result = result.filter(book => book.title.toLowerCase().includes(this.searchText.toLowerCase()));
        console.log(result)
      if (this.queryObject.minPages)
        result = result.filter(book=> book.pages>=this.queryObject.minPages);
        console.log(result)
      if (this.queryObject.maxPages)
        result = result.filter(book=> book.pages<=this.queryObject.maxPages);
      if (this.queryObject.maxPrice)
        result = result.filter(book=> book.price<=this.queryObject.maxPrice);  
      return result;
    }
  },
  methods: {
    showNewBookForm() {
      this.book = Object.assign(this.book, {
        title: "",
        authors: [],
        isbn: "",
        published: "1997-01-10T22:00:00.000Z",
        pages: 0,
        price: 0
      });
      this.formAction = this.addNewBook;
      this.formVisible = true;
    },
    addNewBook() {
      let copy = Object.assign({}, this.book);
      this.books.push(copy);
    },
    removeBook(index) {
      this.filtredBooks.splice(index, 1);
    },
    showUpdateBookForm(index) {
      this.selectedIndex = index;
      Object.assign(this.book, this.filtredBooks[index]);
      this.formAction = this.updateBook;
      this.formVisible = true;
    },
    updateBook(index) {
      Object.assign(this.filtredBooks[this.selectedIndex], this.book);
      this.selectedIndex = -1;
    },
    formInput() {
      this.formAction();
      this.formVisible = false;
    },
    formAction: function() {}
  }
};
</script>

<style scoped>
#app {
  margin: 0;
  padding: 0;
}
</style>