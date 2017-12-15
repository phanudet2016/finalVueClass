<template>
  <div class="hello">
    <div>
      <input type="text" v-model="name" placeholder="NAME">
      <input type="text" v-model="tel" placeholder="TEL">
      <button @click="insert(tel, name)">Add</button>
    </div>

    <hr>

    <table class="table table-hover table-striped" style="margin:auto;">
      <thead>
        <tr>
          <th width="200px" style="text-align: center;">NAME</th>
          <th width="200px" style="text-align: center;">PHONE</th>
          <th width="200px" style="text-align: center;">UPDATE</th>
          <th width="200px" style="text-align: center;">DELETE</th>
        </tr>
      </thead>
      <tbody v-for="phoneBook of phoneBooks" v-bind:key="phoneBook['.key']">
        <tr v-if="updateKey === phoneBook['.key']">
          <td><input type="text" v-model="nameEdit"></td>
          <td><input type="text" v-model="phoneEdit"></td>
          <td>
            <button @click="saveUpdate()">SAVE</button>
            <button @click="cancelUpdate()">cancelSAVE</button>
          </td>
          <td @click="remove(phoneBook['.key'])">DELETE</td>
        </tr>
        <tr v-else>
          <td>{{phoneBook.name}}</td>
          <td>{{phoneBook.phone}}</td>
          <td @click="setUpdate(phoneBook['.key'])">UPDATE</td>
          <td @click="remove(phoneBook['.key'])">DELETE</td>
        </tr>
      </tbody>
      </table>
  </div>
</template>

<script>
import {phoneRef} from './firebase'

export default {
  name: 'HelloWorld',
  data () {
    return {
      name: '',
      tel: '',
      updateKey: '',
      nameEdit: '',
      phoneEdit: ''
    }
  },
  firebase: {
    phoneBooks: phoneRef
  },
  methods: {
    insert () {
      phoneRef.push({
        name: this.name,
        phone: this.tel
      })
    },
    setUpdate (key) {
      this.updateKey = key
      this.nameEdit = this.phoneBooks.find(phoneBooks => phoneBooks['.key'] === key).name
      this.phoneEdit = this.phoneBooks.find(phoneBooks => phoneBooks['.key'] === key).phone
    },
    saveUpdate () {
      phoneRef.child(this.updateKey).update({
        name: this.nameEdit,
        phone: this.phoneEdit
      })
      this.updateKey = ''
    },
    cancelUpdate () {
      this.updateKey = ''
    },
    remove (key) {
      phoneRef.child(key).remove()
    }
  }
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
