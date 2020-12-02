<template>
  <div class="container">
    <!-- <h1>{{ msg }}</h1> -->
    <div class="modal">
      <h1>
        累積銷售金額 <br>
        <p>{{ final }}</p>
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
    <div class="specific">
      <p>SHL <span>{{ SHL }}</span></p>
      <p>Mustela <span>{{ Mustela }}</span></p>
      <p>MWF <span>{{ MWF }}</span></p>
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
      timestamp: '',
      SHL: 0,
      Mustela: 0,
      MWF: 0
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
        'https://spreadsheets.google.com/feeds/cells/1dle84_dm4YR5KIE0E1Dk90WNWsm3LhgRo1YJmseMkvY/1/public/values?alt=json'
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
        })
        console.log(this.result)
        // console.log('t', target)
        console.log(this.result[2].row[this.result[7].row.length].numericValue)
        this.result.forEach(r => {
          if (r.row[r.row.length - 1].numericValue) {
            console.log(r)
            console.log(r.row[r.row.length - 1].numericValue)
            if (r.col === '2' || r.col === '3' || r.col === '4') this.SHL += parseInt(r.row[r.row.length - 1].numericValue)
            if (r.col === '5' || r.col === '6' || r.col === '7') this.Mustela += parseInt(r.row[r.row.length - 1].numericValue)
            if (r.col === '8' || r.col === '9' || r.col === '10') this.MWF += parseInt(r.row[r.row.length - 1].numericValue)
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
// @font-face {
//   @font-face {
//   font-family: 'cwTeXKai';
//   font-style: normal;
//   font-weight: 500;
//   src: url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.eot);
//   src: url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.eot?#iefix) format('embedded-opentype'),
//        url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.woff2) format('woff2'),
//        url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.woff) format('woff'),
//        url(//fonts.gstatic.com/ea/cwtexkai/v3/cwTeXKai-zhonly.ttf) format('truetype');
// }
// }
@import url('https://fonts.googleapis.com/earlyaccess/cwtexyen.css');
@import url('https://fonts.google.com/specimen/Anton#standard-styles');
@import url('https://fonts.googleapis.com/css?family=Bree+Serif');
.container{
  background: url('../assets/bg2.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  // align-items: center;
  // display: flex;
  justify-content: center;
  height: 100vh;
  width: 100%;
  padding: 100px 0 30px 0;
  font-family: cwTeXKai;
  h1{
    // font-family: cwTeXKai,Papyrus;
    font-family: cwtexyen;
    color: #000;
    display: block;
    p{
      margin: 10px 0;
      font-family: "Bree Serif";
      font-size: 80px;
    }
  }
}
.modal{
  border-radius: 5px;
  color: #000;
  font-family: sans-serif;
  line-height: 1.5;
  width: 60%;
  margin: auto;
  max-width: 80%;
  padding: 1rem 2rem;
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.6);
}
.specific{
    display: flex;
    width: 60%;
    margin: auto;
    justify-content: space-between;
    padding: 40px 0;
    // align-items: center;
    p{
      margin: 50px 10px;
      padding: 10px;
      -webkit-backdrop-filter: blur(10px);
      backdrop-filter: blur(10px);
      background-color: rgba(255, 255, 255, 0.6);
      width: 20vw;
      font-family: "Bree Serif";
      font-size: 30px;
      font-weight: 400;
      span{
        display: block;
        font-size: 20px
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
