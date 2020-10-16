<template>
    <input class="items__notes" v-bind:value="note.title"/>
    <button v-on:click="$emit('remove-notes', note.id)">Delete Note</button>
</template>
 
<script>

    export default  {
        name:"Note",
        props:{
            note:{
                type:Object,
                required:true
            }
        }
    }
</script>

<style scoped>
    .items__notes{
        margin:10px 0;
    }
 
</style>    