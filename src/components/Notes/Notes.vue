<template>
    <div class="wrapper__notes">
        <Note
            v-for="note in notes" 
            :key="note"
            v-bind:note="note"
            v-on:remove-notes="removeNotes"
        />
    </div>
</template>
 
<script>
    import Note from '../Note/Note'
    export default  {
        name:"Notes",
        props:['notes'],
        components:{
            Note
        },
        methods:{
            removeNotes (id) {
                console.log(id)
                this.$emit('remove-notes',id)
            }
        }
    }
</script>

<style scoped>
   .wrapper__notes {
       display:flex;
       flex-direction:column;
       flex-basis:45%;
   }
 
</style>