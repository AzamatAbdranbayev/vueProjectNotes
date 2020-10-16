<template>
    <form class="wrapper" @submit.prevent="onSubmit">
        <input class="user__input" v-model="title"/>
        <button 
            class="user__button" 
            type="submit"
            id="btn"
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
        font-family:inherit;
    }
    .user__button {
        width:100px;
        background:#FCA311;
        color:white;
        font-family:inherit;
        color:#000000;
    }
    .wrapper {
        display:flex;
        flex-direction:column;
        flex-basis:45%;
        align-items:center;
    }
 
</style>