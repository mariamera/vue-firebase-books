<template>
  <div id="app" class="container">
        <div class="page-header">
              <h1> Vue.js 2 & Firebase Sample App </h1>
        </div>

        <div  class="panel panel-default">
                <div class="panel-heading">
                  <h3> Add Book</h3>
                </div>
                 <div class="panel-body">
                 <form id="form" class="form-inline" v-on:submit.prevent="addBook">
                 <div class="form-group">
                 <label for="bookTitle"> Title: </label>
                 <input type text id="bookTitle" class="form-control" v-model="newBook.title">
                  <label for="bookTitle"> Author: </label>
                 <input type text id="bookAuthor" class="form-control" v-model="newBook.Author">
                  <label for="bookTitle"> Url: </label>
                 <input type text id="bookUrl" class="form-control" v-model="newBook.url">
                 </div>
                 <input type="submit" class="btn btn-primary" value="Add Book" >
                 </form>
                 </div>
        </div>


        <div class="panel panel-default">
            <div class="panel-heading">
                <h3> Books List </h3>
            </div>
        </div>

         <div class="panel-body">
                <table class=" table table-striped">
                   <thead>
                    <tr>
                      <th>Title</th>
                      <th>Author</th>
                      <th></th>
                    </tr>
                    </thead>
                    <tbody>
                      <tr v-for="book in books">
                        <td><a :href="book.url">{{book.title}}</a></td>
                        <td>{{book.Author}}</td>
                        <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
                      </tr>
                    </tbody>
                </table>
            </div>
  </div>
</template>

<script>

import Hello from './components/Hello'

import Firebase from 'firebase'


let config = {
    apiKey: "AIzaSyBMcvZvwK3r2KMgP0R7C8KzYgA1wt-w8TE",
    authDomain: "vuejs-firebase-01-f4490.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-01-f4490.firebaseio.com",
    storageBucket: "vuejs-firebase-01-f4490.appspot.com",
    messagingSenderId: "559473795131"
  };

let app = Firebase.initializeApp(config);
let db = app.database();
let booksRef = db.ref('books');
export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data(){
    return {
      newBook: {
        title: '',
        Author: '',
        url: ''
      }
    }
  },
  methods: {
      addBook: function(){
        booksRef.push(this.newBook);
        this.newBook.title = '';
        this.newBook.Author = '';
        this.newBook.url = '';
      },
      removeBook: function(book){
          booksRef.child(book['.key']).remove();

      }

  }
}
</script>



<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
</style>
