<template>
    <form class="wrapper" @submit.prevent="onSubmit">
        <input class="user__input" v-model="title"/>
        <button 
            class="user__button" 
            type="submit"
        >
            Добавить запись
        </button>
    </form>  
</template>

<script>

    export default  {
        name:"InputUser",
        data(){
            return {
                title:""
            }
        },
        methods: {
            onSubmit() {
                if(this.title.trim()) {
                    const newNotes = {
                        id:Date.now(),
                        title:this.title,
                        completed:false
                    }
                    this.$emit("add-notes",newNotes)
                }
            }
        }
    }
</script>

<style scoped>
    .user__input {
        width:300px;
        height:300px;
    }
    .user__button {
        width:70px;
        background:#007BFE;
        color:white;
    }
    .wrapper {
        display:flex;
        flex-direction:column;
        flex-basis:45%;
    }
 
</style>