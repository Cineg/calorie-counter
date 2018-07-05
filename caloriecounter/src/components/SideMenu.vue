<template>
    <nav> 
        <router-link to="/dashboard" active-class="active">
            <div class="icon" ><i class="fas fa-home"></i></div>
        </router-link>
        <router-link to="/add" active-class="active">
            <div class="icon" > <i class="fas fa-plus"> </i></div>
        </router-link>
        <router-link to="/about" active-class="active">
            <div class="icon"><i class="fas fa-question-circle"></i></div>
        </router-link>
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
        grid-area: sidemenu;
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
    .active .icon{
        background-image: linear-gradient(to left bottom, #adeff3, #98e8ed, #81e1e7, #68d9e1, #48d2db);
        color: #fefefe;
    }

    a{
        text-decoration: none;
    }

    @media screen and (max-width: 1000px){
        nav{
            width: 100vw;
            height: 80px;
            display: flex;
            padding: 0;
            justify-content: center;
        }

        .icon{
            width: 25vw;
            height: 80px;
        }
    }

    @media screen and (min-width: 1000px) and (max-width: 1600px){
        nav{
            width: 120px;
        }

        .icon{
            height: 100px;
        }
    }
</style>
