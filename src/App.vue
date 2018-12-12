<template>
  <div>
    Status: <span v-if="status">Signed in.</span><span v-else>Signed out.</span> <br>
    <input v-model="username" placeholder="Username" type="text"><br>
    <input v-model="password" placeholder="Password" type="text"><br>
    <button @click="createUser()">Create User</button>
    <button v-if="!status" @click="signIn()">Login</button>
    <button v-else @click="signOut()">Logout</button>
    <br><br>
    Or login with: <br>
    <button @click="signInWithGoogle">Google</button>
    <button @click="sayHello">Twitter</button>
    <button @click="signInWithGithub">Github</button>
    <button @click="signInWithFacebook">Facebook</button> <br><br>
    Display name: {{ userData.displayName }} <br>
    Email: {{ userData.email }} <br>
    Photo: <img :src="userData.photoURL" alt="">
  </div>
</template>

<script>
import Firebase from 'firebase'

var config = {
  apiKey: "AIzaSyCGCgCTu9OwUZbprgmqH9-KCTp0jjKtCFs",
  authDomain: "vuevuexprettygirlsapi.firebaseapp.com",
  databaseURL: "https://vuevuexprettygirlsapi.firebaseio.com",
  projectId: "vuevuexprettygirlsapi",
  storageBucket: "vuevuexprettygirlsapi.appspot.com",
  messagingSenderId: "187458410833"
};

let firebase = Firebase.initializeApp(config);
// const firebaseAuth = firebaseApp.firestore()

export default {
  name: 'App',
  created: function() {
    let self = this

    firebase.auth().onAuthStateChanged(function(user) {
      if (user) {
        // User is signed in.
        self.userData = user

        var displayName = user.displayName;
        var email = user.email;
        var emailVerified = user.emailVerified;
        var photoURL = user.photoURL;
        var isAnonymous = user.isAnonymous;
        var uid = user.uid;
        var providerData = user.providerData;
        
        self.status = true
      } else {
        // User is signed out.
        self.status = false
      }
    })
  },
  data: function () {
    return {
      userData: '',
      username: 'username@mail.com',
      password: 'password',
      status: false,
    }
  },
  methods: {
    sayHello: function () {
      alert('This is hello!')
    },
    createUser: function () {
      let email = this.username
      let password = this.password

      firebase.auth().createUserWithEmailAndPassword(email, password).then(() => {
        alert('Success')
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;

        alert('Error')
      });
    },
    signIn: function () {
      let email = this.username
      let password = this.password

      firebase.auth().signInWithEmailAndPassword(email, password).then(() => {
        alert('Success')
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;

        alert('Error')
      });
    },
    signInWithGoogle: function () {
      let self = this
      var provider = new Firebase.auth.GoogleAuthProvider();
      firebase.auth().signInWithPopup(provider).then(function(result) {
        // This gives you a GitHub Access Token. You can use it to access the GitHub API.
        var token = result.credential.accessToken;
        // The signed-in user info.
        var user = result.user;

        alert('Google success')
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        // ...
      });

    },
    signInWithFacebook: function () {
      let self = this
      var provider = new Firebase.auth.FacebookAuthProvider();
      firebase.auth().signInWithPopup(provider).then(function(result) {
        // This gives you a GitHub Access Token. You can use it to access the GitHub API.
        var token = result.credential.accessToken;
        // The signed-in user info.
        var user = result.user;

        alert('Facebook success')
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        // ...
        console.log(error)
      });

    },
    signInWithGithub: function () {
      var provider = new Firebase.auth.GithubAuthProvider();
      firebase.auth().signInWithPopup(provider).then(function(result) {
        // This gives you a GitHub Access Token. You can use it to access the GitHub API.
        var token = result.credential.accessToken;
        // The signed-in user info.
        var user = result.user;

        alert('Github success')
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;

        if (credential) {
          firebase.auth().currentUser.linkWithCredential(credential)
        }

        console.log(error)
      });
    },
    signOut: function () { 
      firebase.auth().signOut()
      this.userData = '';
      alert('Success')
    }
  }
}
</script>

<style>
</style>
