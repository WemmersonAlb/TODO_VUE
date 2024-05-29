<template>
    <div id="feedback" :class="tipoStatus">
        <h2>{{titleMsg}}</h2>
        <p>{{msg}}</p>
    </div>
</template>
<script>
export default {
    name:'Message',
    props:{
        msg: String,
        tipoStatus: String,
        titleMsg: String
    }
}
</script>
<style scoped>
    .delete{
        background: rgb(90, 21, 21);
        color: rgb(212, 73, 73);
    }
    .update{
        background: rgb(5, 47, 109);
        color: rgb(94, 132, 190);
    }
    .create{
        background: rgb(4, 77, 2);
        color: rgb(113, 212, 109);
    }
    #feedback{
        position: fixed;
        bottom:100px;
        left:50px;
        padding: 15px;
        width: 300px;
        border-radius: 2px; 
        z-index: 1000;
        box-shadow: 2px 6px 8px rgba(0, 0, 0, 0.562);
    }
</style>