<template>
    <div>
        <p v-if="bookList.length==0" class="alert">
            Книги відсутні
        </p>
        
        <table v-if="bookList.length>0">
            <tr>
                <th>#</th>
                <th v-on:click="sort('title')">  Назва </th>
                <th v-on:click="sortAuthor()"> Автор  </th>
                <th v-on:click="sort('published')">  Дата </th>
                <th v-on:click="sort('pages')"> Сторінок </th>
                <th v-on:click="sort('price')"> Ціна </th>
                <th></th>
            </tr>
            <bookTableRow v-for="(book,index) in bookList" 
                v-bind:key="book._id" 
                v-bind="{book,index}"
                @remove="remove"
                @update="update" 
            >             
            </bookTableRow>
        </table>
    </div> 
</template>

<script>
import bookTableRow from "./bookTableRow";

export default {
    name:"bookTable",
    props:{
        bookList:Array,
    },
    data(){
        return{
           books: this.bookList
        }
    },
    components:{
        bookTableRow
    },
    methods:{
        sort(field){
           this.bookList.sort((b1,b2)=> b1[field]>=b2[field]?1:-1);
        },
        sortAuthor(){
            this.bookList.sort((b1,b2)=>b1.authors.join(",")>=b2.authors.join(",")?1:-1);
        }, 
        remove(index){
            this.$emit("remove",index);
        },  
        update(index){
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    .alert{
        background: yellow;
        color: crimson;
    }

    table, table td{
        border-collapse: collapse;
        border: 1px solid black;
    }
</style>