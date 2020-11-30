<template>
  <div class="container">
    <!-- <h1>{{ msg }}</h1> -->
    <h1>
      累積銷售金額 {{ result[0].content.$t }}
    </h1>
    <div class="modal">
      <table>
        <tr>
          <td v-for="row of result" :key="row.id">
            {{ row.content.$t }}
          </td>
        </tr>
      </table>
    </div>
    <!-- <h1>The result is {{ result }}</h1> -->
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      result: {},
      lastrow: null
    }
  },
  mounted () {
    this.getResult()
  },
  methods: {
    getResult () {
      axios.get(
        'https://spreadsheets.google.com/feeds/cells/1dhrFx6Er7aUu31in5xoGUwAbzAPCsAyKIuthwio2fXY/1/public/values?alt=json'
      ).then((res) => (this.result = res.data.feed.entry))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container{
  background: url('../assets/bg.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  align-items: center;
  display: flex;
  justify-content: center;
  height: 100%;
  width: 100%;
  padding: 30px 0;
}
.modal{
  border-radius: 5px;
  color: #fff;
  font-family: sans-serif;
  line-height: 1.5;
  max-width: 50%;
  padding: 1rem 2rem;
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  background-color: rgba(0, 0, 0, 0.4);
}
h3 {
  margin: 40px 0 0;
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
