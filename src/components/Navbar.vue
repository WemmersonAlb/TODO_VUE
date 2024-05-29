<template>
    <nav>
        <img src="/img/logo.png" alt="Logo The Check List">
        <h1>The Check List</h1>
    </nav>
</template>
<script>
export default {
    name:'Navbar'
}
</script>
<style scoped>
    h1{
        color: #D9D9D9;
        font-family: "Inter", sans-serif;
        font-size: 40px;
        margin-left: 10px;
    }
    nav{
        width: 100%;
        margin: 0 auto;
        background: #222;
        box-shadow: 0 5px 5px rgba(0, 0, 0, 0.6)  ;
        height: 100px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        padding-left: 30px;
    }
    /* Pequenas telas e mobile */
    @media (max-width:600px){
        img{
            height: 30px;
        }
        h1{
            font-size: 20px;
        }
        nav{
            height: 50px;
        }
    }
</style>