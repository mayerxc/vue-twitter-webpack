<template>
    <div>
        <h1>{{title}}</h1>
        <h3>{{msg}}</h3>
        <input type="text" v-model="title">
        <h1>This button changes the title to something else:</h1>
        <button v-on:click="addNewTitle">Submit</button>
    </div>
</template>

<script>
export default {
    name: 'AnotherComponent',
    props: {
        msg: String,
    },
    data: function(){
        return {
            title: 'this is a test!'
        }
    },
    methods: {
        addNewTitle: function(){
            this.title = "changing from the button";
        }
    }

};
</script>

<style scoped>
h1 {
    color:darkred;
}

</style>