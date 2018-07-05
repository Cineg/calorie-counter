<template>
    <nav> 
        <div class="icon active"><i class="fas fa-home"></i></div>
        <div class="icon"> <i class="fas fa-plus"> </i></div>
        <div class="icon"> <i class="fas fa-user"></i></div>
        <div class="icon"><i class="fas fa-question-circle"></i></div>
    </nav>
</template>

<script>
export default {
    name: 'sidemenu'
}
</script>

<style>
    nav{
        width: 160px;
        height: 100vh;
        background-color: #EBE9E9;
        padding: 160px 0;
    }

    .icon{
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 40px;
        color: rgba(112, 112, 112, 0.5);
        cursor: pointer;
        width: 100%;
        height: 120px;

    }
    .active{
        background-image: linear-gradient(to left bottom, #adeff3, #98e8ed, #81e1e7, #68d9e1, #48d2db);
        color: #fefefe;
    }


    @media screen and (max-width: 800px){
        nav{
            width: 100vw;
            height: 80px;
            display: flex;
            padding: 0;
            justify-content: center;
        }

        .icon{
            width: 120px;
            height: 80px;
        }
    }
</style>
