<template>
    <div class="items__notes">
        <input v-bind:value="note.title">
        <button class="notes__button" v-on:click="$emit('remove-notes', note.id)">Delete Note</button>
    </div>
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
        display:flex;
        justify-content:space-between;
    }
    .notes__button {
        width:140px;
        color:white;
        background:#D73A20;
        border:none;
        font-family:inherit;
    }
    .items__notes > input:hover,
    .notes__button:hover {
        box-shadow: 0 0 2px 3px #E5E5E5;
    }
    .items__notes >input {
        font-family:inherit;
    }
</style>    