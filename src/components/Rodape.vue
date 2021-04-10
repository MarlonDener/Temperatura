<template>
    <footer>{{text}}</footer>
</template>

<script>
export default{
    name:'Rodape',
    props:{
        text: String
    }
}

</script>

<style scoped>
    footer{
        padding: 20px 2%;
        width: 100%;
        background-color: rgba(5, 5, 5, 0.164);
        color:#fff;
        text-align: center;
        position: fixed;
        bottom: 0;
        box-shadow: 10px 5px 7px 2px rgba(0,0,0,0.5);
        border-top: 1px solid rgba(255, 255, 255, 0.116);
    }
</style>
