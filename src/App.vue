<template>
  <div id="app">
    <table>
      <thead>
        <tr>
          <th>Személy</th>
          <th>Magasság</th>
          <th>Ár</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="statue in statues" v-bind:key="statue.id">
          <td>{{ statue.person }}</td>
          <td>{{ statue.height }}</td>
          <td>{{ statue.price }}</td>
          <td>
            <button @click="deleteStatue(statue.id)">Törlés</button>
            <button @click="editStatue(statue.id)">Szerkesztés</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="text" v-model="statue.person">
          </td>
          <td>
            <input type="number" v-model="statue.height">
          </td>
          <td>
            <input type="number" v-model="statue.price">
          </td>
          <td>
            <button v-if="mod_new" @click="newStatue" >Létrehoz</button>
            <button v-if="!mod_new" @click="saveStatue" >Mentés</button>
            <button v-if="!mod_new" @click="cancelEdit" >Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      mod_new: true,
      statue: {
        id: null,
        person: '',
        height: '',
        price: false
      },
      statues: []
    }
  },
  methods: {
    async loadData () {
     let Response = await fetch('http://127.0.0.1:8000/api/statues')
     let data = await Response.json()
     this.statues = data
    },
    async deleteStatue(id) {
      await fetch(`http://127.0.0.1:8000/api/statues/${id}`, {
        method: 'DELETE'
      })
      await this.loadData()
    },
    async newStatue() {
     await fetch('http://127.0.0.1:8000/api/statues', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadData()
     this.reset()
    },
    async saveStatue() {
     await fetch(`http://127.0.0.1:8000/api/statues/${this.statue.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadData()
     this.reset()
    },
    async editStatue(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`)
      let data = await Response.json()
      this.statue = {...data};
      this.mod_new = false
    },
    cancelEdit () {
      this.reset()
    },
    reset() {
      this.statue = {
        id: null,
        person: '',
        height: '',
        price: false
      }
      this.mod_new = true
    }
  },
  mounted() {
    this.loadData()
  }
}
</script>
<style>
body{
  background-color: indigo;
  color: aliceblue;
  font-size: 14pt;
  font-family: Georgia, Times, 'Times New Roman', serif;
}
table{
  margin: 2% auto;
}
th,td{
  width: 25%;
  text-align: center;
}
th{
  text-decoration:underline;
}
button{
  all: unset;
  border: 1px solid white;
  color: aquamarine;
  background-color: green;
  padding: 2px;
  margin: 1px;
}
button:hover{
  border: 1px solid black;
  color: plum;
  background-color: purple;
}
</style>