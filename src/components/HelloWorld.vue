<template>
  <div class='hello'>
    <div>
      <input type="text" v-model="name" placeholder="NAME">
      <input type="text" v-model="tel" placeholder="TEL">
      <input type="text" v-model="gunclass" placeholder="Class">
      <input type="text" v-model="typegun" placeholder="Competition typeL">
      <button @click="insertToContact(tel, name,gunclass,typegun)">Add</button>
    </div>

   

    <ul :key="key" v-for="(contact, key) in contacts">
      <div v-if="updateKey === key">
        <input type="text" v-model="updateName" placeholder="NAME">
        <input type="text" v-model="updateTel" placeholder="TEL">
        <input type="text" v-model="updateGunclass" placeholder="Class">
        <input type="text" v-model="updateTypegun" placeholder="Competition typeL">
        <button @click="updateContact(updateTel, updateName,updateGunclass,updateTypegun)">Save</button>
      </div>
      <li v-else>
        {{contact.name}} : {{contact.tel}} : {{contact.gunclass}} : {{contact.typegun}}
        <button @click="setUpdateContact(key, contact)">Update</button>
        <button @click="deleteContact(key)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import * as firebase from 'firebase'
var firebaseConfig = {
    apiKey: "AIzaSyBihnRYIuEePsdLR_X-ZtUcLFHmJfvFEuc",
    authDomain: "ppproject-19364.firebaseapp.com",
    databaseURL: "https://ppproject-19364.firebaseio.com",
    projectId: "ppproject-19364",
    storageBucket: "ppproject-19364.appspot.com",
    messagingSenderId: "854439128758",
    appId: "1:854439128758:web:71e000a4edeee5215dc017",
    measurementId: "G-4J0E00B2FM"
  };
  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);
  firebase.analytics();
var database = firebase.database()
var contactRef = database.ref('/contacts')
export default {
  name: 'HelloWorld',
  data () {
    return {
      contacts: {},
      tel: '',
      name: '',
      gunclass:'',
      typegun:'',
      updateTel: '',
      updateName: '',
      updateKey: '',
      updateGunclass: '',
      updateTypegun: '',
    }
  },
  methods: {
    insertToContact (tel, name,gunclass,typegun) {
      let data = {
        tel: tel,
        name: name,
        gunclass: gunclass,
        typegun: typegun
      }
      contactRef.push(data)
      this.tel = ''
      this.name = ''
      this.gunclass = ''
      this.typegun = ''
    },
    setUpdateContact (key, contact) {
      this.updateKey = key
      this.updateTel = contact.tel
      this.updateName = contact.name
      this.updateGunclass = contact.gunclass
      this.updateTypegun = contact.typegun
    },
    updateContact (tel, name,gunclass,typegun) {
      contactRef.child(this.updateKey).update({
        tel: tel,
        name: name,
        gunclass: gunclass,
        typegun: typegun
      })
      this.updateKey = ''
      this.updateTel = ''
      this.updateName = ''
      this.updateGunclass = ''
      this.updateTypegun = ''
    },
    deleteContact (key) {
      contactRef.child(key).remove()
    }
  },
  mounted () {
    contactRef.on('value', (snapshot) => {
      this.contacts = snapshot.val()
    })
  }
}
</script>

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