<script setup>
import TopNavbar from './components/TopNavbar.vue'
import MainList from './components/MainList.vue'

// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "AIzaSyCASwg2p_P-h4fpJ1YbDujHJUaRLbc8wHk",
  authDomain: "vue-crud-a65dd.firebaseapp.com",
  databaseURL: "https://vue-crud-a65dd-default-rtdb.firebaseio.com",
  projectId: "vue-crud-a65dd",
  storageBucket: "vue-crud-a65dd.appspot.com",
  messagingSenderId: "832029862316",
  appId: "1:832029862316:web:75c3f5e2b0fd736feef209"
};

// Initialize Firebase
const firebaseApp = initializeApp(firebaseConfig);

</script>

<template>
  <div>
    <TopNavbar />
    <MainList />
  </div>
</template>
