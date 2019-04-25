<template>
  <div class="info" v-if="user">
    <p>Hello: {{ user.displayName }}</p> 
    <p>Your email: {{ user.email }}</p>  
    <span>Your Avatar:</span><img :src="user.photoURL" alt="avatar" style="width: 30px; height: 30px; border-radius: 50%;">
    <button @click="signOut">Sign Out</button>
  </div>
  <div class="login" v-else>
    <button @click="signInWithGoogle">Sign in with Google</button>
  </div>
</template>

<script>
import axios from "axios";
import firebase from "firebase";

 var config = {
    // Import config from firebase: https://console.firebase.google.com => choose project => project overview => add app
  };
firebase.initializeApp(config);
export default {
  name: 'HelloWorld',
  data () {
    return {
      user: null,
    }
  },
  created() {
    this.initFirebaseAuth();
  },
  methods: {
    signInWithGoogle: function() {
      const provider = new firebase.auth.GoogleAuthProvider()
      firebase.auth().signInWithPopup(provider);
    },
    initFirebaseAuth() {
      firebase.auth().onAuthStateChanged(this.authStateObserver)
    },
    authStateObserver(user) {
      console.log('user', user);
      this.user = user;
    },
    signOut: function() {
      firebase.auth().signOut().then(() => {
        this.user = null;
      }).catch(err => console.log(error))
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
