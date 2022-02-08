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
        <tr>
          <td>{{ statue.person }}</td>
          <td>{{ statue.height }}</td>
          <td>{{ statue.price }}</td>
          <td>
            <button>Törlés</button>
            <button>Szerkesztés</button>
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
      statue: {
        id: null,
        person: '',
        height: '',
        price: false
      },
      statues: []
    }
  },
}
</script>
<style>
</style>