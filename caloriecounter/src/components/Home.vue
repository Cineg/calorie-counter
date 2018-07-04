<template>
    <div class="wrapper">
    <header>
        <h1>kCalculator</h1>
        <p>Policz swoje kalorie bez większych problemów! Wyszukaj, dodaj, wybierz ilość i gotowe!  </p>
        <router-link to="/dashboard">
            <button>Sprawdź!</button>
        </router-link>
    </header>
    </div>
</template>

<script>
export default {
    name: 'home',
}
</script>

<style scoped>
    .wrapper{
        width: 100vw;
        height: 100vh;
        background: url('../assets/bgdesktop.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position: top right;
    }
    header{
        margin: 0 auto;
        padding-top: 200px;
        padding-left: 250px;
        color: #000;
    }
    h1{
        font-size: 80px;
        font-weight: 700;
    }
    p{
        margin-top: 85px;
        width: 760px;
        font-size: 26px;
        margin-bottom: 200px;
    }
    button{
        background: #c9ffb4; /* Old browsers */
        background: -moz-linear-gradient(45deg, #c9ffb4 0%, #cff9be 100%); /* FF3.6-15 */
        background: -webkit-linear-gradient(45deg, #c9ffb4 0%,#cff9be 100%); /* Chrome10-25,Safari5.1-6 */
        background: linear-gradient(45deg, #c9ffb4 0%,#cff9be 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
        filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#c9ffb4', endColorstr='#cff9be',GradientType=1 ); /* IE6-9 fallback on horizontal gradient */
        border: 0;
        border-radius: 15px;
        width: 380px;
        height: 120px;
        font-size: 40px;
        -webkit-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
        -moz-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
        box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
        cursor: pointer;
    }

    button:focus{
        outline: 0;
    }

    @media screen and (max-width: 1024px){
        .wrapper{
            height: 100vh;
            width: 100vw;
            background: url('../assets/bgmobile.svg');
            background-size: contain;
            background-repeat: no-repeat;
            background-position: bottom;
        }
        header{
            padding-top: 75px;
            padding-left: 20px;
            padding-right: 20px;
            margin: 0 auto;
            text-align: center;
        }
        h1{
            font-size: 40px;
            font-weight: 700;
        }
        p{
            margin-top: 40px;
            font-size: 15px;
            width: 100%;
            margin-bottom: 55px;
        }
        
        button{
            background: #c9ffb4; /* Old browsers */
            background: -moz-linear-gradient(45deg, #c9ffb4 0%, #cff9be 100%); /* FF3.6-15 */
            background: -webkit-linear-gradient(45deg, #c9ffb4 0%,#cff9be 100%); /* Chrome10-25,Safari5.1-6 */
            background: linear-gradient(45deg, #c9ffb4 0%,#cff9be 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
            filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#c9ffb4', endColorstr='#cff9be',GradientType=1 ); /* IE6-9 fallback on horizontal gradient */
            border: 0;
            border-radius: 15px;
            width: 190px;
            height: 60px;
            font-size: 21px;
            -webkit-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
            -moz-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
            box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
            cursor: pointer;
        }
       
    }

</style>
