<template>
    <div>

    </div>
</template>

<script>
export default {
    name: 'Search',
    props: {
        msg: String,
    },
    data: function(){
        return {
            title: 'this is a test!',
            
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