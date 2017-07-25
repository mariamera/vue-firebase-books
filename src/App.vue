<template>
  <div id="app" class="container">
        <div class="page-header">
              <h1> Vue.js 2 & Firebase Sample App </h1>
        </div>


<md-tabs md-fixed>
  <md-tab id="movies" md-label="Add Book">
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
  </md-tab>

  <md-tab id="music" md-label="All Books">
      <div class="panel panel-default">
            <div class="panel-heading">
               <div class="row">
      <div class="col-xs-6"><h3> Books List </h3></div>
      <div class="col-xs-6">
         <md-input-container>
    <label>Lookup Books</label>
    <md-input v-model="lookup"></md-input>
  </md-input-container>
      </div>
</div>
<!-- .includes("s") -->
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
                      <tr v-for="book in filtrarUsuarios">

                        <td><a :href="book.url">{{book.title}}</a></td>
                        <td>{{book.Author}}</td>
                        <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>

                      </tr>
                    </tbody>

                </table>
            </div>

  </md-tab>

  <md-tab id="books" md-label="Categories">
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Deserunt dolorum quas.</p>
  </md-tab>

  <md-tab id="pictures" md-label="Contact Us">


<form novalidate @submit.stop.prevent="submit">
  <md-input-container>
    <label>Nombre</label>
    <md-input v-model="nombre" required></md-input>
  </md-input-container>

  <md-input-container>
    <label>Telefono</label>
    <md-input v-model="telefono" required ></md-input>
  </md-input-container>

  <md-input-container>
    <label>Email</label>
    <md-input v-model="email" type="Email" required></md-input>
  </md-input-container>

  <md-input-container>
    <label>Mensaje</label>
    <md-textarea v-model="mensaje" required></md-textarea>
  </md-input-container>

<md-button @click.native="submit" class="md-raised md-primary">Submit</md-button>
</form>



  </md-tab>
</md-tabs>






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
      },
       lookup: '',
       nombre: '',
       telefono: '',
       email: '',
       mensaje: '',
    }
  },
    computed: {
        filtrarUsuarios() {
            return this.books.filter((book) =>{
              return book.title.includes(this.lookup);
            }) ;
            //booksRef.child(book['.key']).title.includes(this.busqueda)
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

      },
      elegirGanador:function(){
        console.log("hola");
      },
      submit: function() {
        var body = "Nombre: " + this.nombre +"\n\n\n     telefono: " + this.telefono + "\n     Email: " + this.email + "\n\n      MENSAJE: \n" + this.mensaje;
        var info = "mailto:test@example.com?subject=subject&body= " + body;
          window.open(info);
      },


  },

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
