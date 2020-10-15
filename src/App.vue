<template>
  <InputUser @add-notes="addNotes"/>
</template>

<script>
  import InputUser from './components/InputUser/InputUser';
  import axios from 'axios'

  export default {
    name: 'App',
    components: {
      InputUser
    },
    data() {
      return {
        notes:[

        ]
      }
    },
    methods:{
      async addNotes (notes) {
        let idResponse = "";
        await axios({
          method:"post",
          url:"https://api.jsonbin.io/b/",
          data: notes,
          headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
        })
        .then(response=>{
          idResponse = response.data.id
          console.log("response ",response)
          console.log("idResponse POST ",idResponse)
        })
        await axios({
          method:"get",
          url:"https://api.jsonbin.io/b/"+idResponse,
          headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
        })
        .then(response=>{
          console.log("idResponse GET",idResponse)
          this.notes.push(response.data)
          })
        .catch(error=>{
          console.log(error)
          console.log("idResponse GET ERROR",idResponse)
        })
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>



