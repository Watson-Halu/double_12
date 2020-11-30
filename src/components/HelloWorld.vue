<template>
  <div class="container">
    <!-- <h1>{{ msg }}</h1> -->
    <div class="modal">
      <h1>
        累積銷售金額 <br>{{ final }}
      </h1>
      <!-- <table>
        <tr>
          <td v-for="(item,i) in result" :key="i">
            <p v-for="(child,j) in item.row" :key="j">
              {{ child.$t }}
            </p>
          </td>
        </tr>
      </table> -->
      <h1>
        現在時間：{{ timestamp }}
      </h1>
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
      result: [],
      final: 0,
      timestamp: ''
    }
  },
  created () {
    setInterval(this.getNow, 1000)
  },
  mounted () {
    this.getResult()
  },
  methods: {
    getResult () {
      axios.get(
        'https://spreadsheets.google.com/feeds/cells/1dhrFx6Er7aUu31in5xoGUwAbzAPCsAyKIuthwio2fXY/1/public/values?alt=json'
      ).then(res => {
        res.data.feed.entry.forEach(d => {
          var procedure = true
          this.result.forEach((e, i) => {
            if (e.col === d.gs$cell.col) {
              this.result[i].row.push(d.gs$cell)
              procedure = false
            }
          })
          if (procedure) {
            this.result.push({
              col: d.gs$cell.col,
              row: [d.gs$cell]
            })
          }
          console.log('this.result', this.result)
        })
        console.log(this.result)
        this.result.forEach(r => {
          if (r.row[r.row.length - 1].numericValue) {
            console.log(r)
            this.final += parseInt(r.row[r.row.length - 1].numericValue)
          }
        })
        console.log('結果', this.final)
      })
    },
    getNow () {
      const today = new Date()
      const date = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate()
      const time = today.getHours() + ':' + today.getMinutes() + ':' + today.getSeconds()
      const dateTime = date + ' ' + time
      this.timestamp = dateTime
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@font-face {
  @font-face {
  font-family: 'cwTeXKai';
  font-style: normal;
  font-weight: 500;
  src: url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.eot);
  src: url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.eot?#iefix) format('embedded-opentype'),
       url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.woff2) format('woff2'),
       url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.woff) format('woff'),
       url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.ttf) format('truetype');
}
}
// @import url();
.container{
  background: url('../assets/bg4.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  align-items: center;
  display: flex;
  justify-content: center;
  height: 100vh;
  width: 100%;
  padding: 30px 0;
  font-family: cwTeXKai;
  h1{
    font-family: cwTeXKai;
    color: #fff;
    display: block;
  }
}
.modal{
  border-radius: 5px;
  color: #fff;
  font-family: sans-serif;
  line-height: 1.5;
  max-width: 80%;
  padding: 1rem 2rem;
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  background-color: rgba(0, 0, 0, 0.6);
  table{
    tr{
      border:1px solid blue
    }
    td{
      border:1px solid red;
    }
  }
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
