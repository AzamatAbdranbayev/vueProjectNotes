<template>
  <div class="container">
    <InputUser @add-notes="addNotes"/>
    <Notes 
      v-bind:notes="notes"
      @remove-notes="removeNote"
    />
  </div>
</template>

<script>
  import InputUser from './components/InputUser/InputUser';
  import Notes from './components/Notes/Notes';
  import axios from 'axios'

  export default {
    created () {
        axios({
        method:"get",
        url:"https://api.jsonbin.io/e/collection/5f89bc78302a837e957a1ff7/all-bins",
        headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
      })
      .then(response=>{
          const records = response.data.records
          records.map(el=>{
            axios({
              method:"get",
              url:"https://api.jsonbin.io/b/"+el.id,
              headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
            })
            .then(response=>{
              response.data.id = el.id
              this.notes.push(response.data)
            })
            .catch(e=>console.log("array error",e))
          })
        })
        .catch(e=>console.log("created error",e))
    },
    name: 'App',
    components: {
      InputUser,
      Notes
    },
    data() {
      return {
        notes:[]
      }
    },
    methods:{
      async addNotes (notes) {
        await axios({
          method:"post",
          url:"https://api.jsonbin.io/b/",
          data: notes,
          headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5.",
                  "collection-id":"5f89bc78302a837e957a1ff7"}
        })
        .then(response=>{
          this.notes.push(response.data.data)
        })
      },
      removeNote (id) {
        console.log("before ", this.notes)
        this.notes = this.notes.filter(el=>el.id !== id)
        console.log("after ", this.notes)
        axios({
              method:"delete",
              url:"https://api.jsonbin.io/b/"+id,
              headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
            })
      }
    }
  }
</script>

<style>
  .container {
    max-width:1200px;
    margin:0 auto;
    display:flex;
    justify-content:space-between;
  }
</style>



