<template>
    <section>
        <side-menu/>
        <div class="info">
            <h4>Autor: Marcin Michałek</h4>
            <a href="mailto:michalekmrcn@gmail.com">
                <i class="far fa-envelope"></i>michalekmrcn@gmail.com
            </a>
        </div>
    </section>
</template>

<script>
import SideMenu from './SideMenu'

export default {
    name: 'about',
    components: {
        SideMenu,
    },
}
</script>

<style scoped>
    section{
        display: flex;
        background: url('../assets/bgdesktop.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position: top right;
    }

    .info{
        margin-left: 10vw;
        margin-top: 30vh;
        font-size: 140%;
        display: flex;
        flex-direction: column;
    }

    h4{
        font-size: 60px;
        margin-bottom: 15px;
    }
    a{
        text-decoration: none;
        color: #000;
        font-size: 18px;
    }

     @media screen and (max-width: 1000px){
         section{
            height: 100vh;
            flex-direction: column;
            background: url('../assets/bgmobile.svg');
            background-size: cover;
            background-repeat: no-repeat;
            background-position: bottom;
         }
        h4{
            font-size: 40px;
        }
     }
</style>
