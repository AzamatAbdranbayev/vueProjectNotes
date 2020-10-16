<template>
  <Loader v-if="loading"/>
  <div class="progressBar" v-bind:style="getProgress(notes.length*10)">{{notes.length + ' / 10'}}</div>
  <div class="container" v-if="!loading">
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
  import Loader from './components/Loader/Loader';
  import axios from 'axios';

  export default {
    created () {
        axios({
        method:"get",
        url:"https://api.jsonbin.io/e/collection/5f89bc78302a837e957a1ff7/all-bins",
        headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
      })
      .then(response=>{
          const records = response.data.records;
          records.map(el=>{
            axios({
              method:"get",
              url:"https://api.jsonbin.io/b/"+el.id,
              headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
            })
            .then(response=>{
              response.data.id = el.id;
              this.notes.push(response.data);
              this.loading =false;
            })
            .catch(e=>alert("unable to connect to JSONbin.io"+e))
          })
        })
        .catch(e=>alert("unable to connect to JSONbin.io"+e))
    },
    name: 'App',
    components: {
      InputUser,
      Notes,
      Loader,
    },
    data() {
      return {
        notes:[],
        loading:true
      }
    },
    methods:{
      async addNotes (notes) {
        if(this.notes.length <=9) {
          await axios({
            method:"post",
            url:"https://api.jsonbin.io/b/",
            data: notes,
            headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5.",
                    "collection-id":"5f89bc78302a837e957a1ff7"}
          })
          .then(response=>{
            this.notes.push(response.data.data);
          })
        }
        else {
          alert("Вы достигли максимума");
        }
        
      },
      removeNote (id) {
        this.notes = this.notes.filter(el=>el.id !== id);
        axios({
              method:"delete",
              url:"https://api.jsonbin.io/b/"+id,
              headers:{"secret-key":"$2b$10$NXA4SWMOk8DOUK6/f30Ylu/CvqY.tumhGMcm1YBJX2MjUND4WtC5."}
            })
      },
      getProgress: function (width) {
        return {
          'width':width+'%',
          'background-color':'#FCA311',
          'height':'30px',
          'min-width':'60px',
          'margin':'15px 0'
        }
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



