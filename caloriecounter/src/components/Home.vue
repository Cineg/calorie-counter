<template>
    <div class="wrapper">
    <header>
        <h1>kCalculator</h1>
        <p>Policz swoje kalorie bez większych problemów! Wyszukaj, dodaj, wybierz ilość i gotowe!  </p>
        <router-link to="/dashboard">
            <button>Sprawdź!</button>
        </router-link>
    </header>


    <div class="bar bar1"></div>
    <div class="bar bar2"></div>
    <div class="bar bar3"></div>
    <div class="bar bar4"></div>
    <div class="bar bar5"></div>
    </div>
</template>

<script>
export default {
    name: 'home',
}
</script>

<style scoped>
    .wrapper{
        width: 1920px;
        height: 1080px;
    }
    header{
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

    .bar{
        width: 210px;
        height: 1500px;
    }
    .bar1{
        position: absolute;
        right: 319px;
        top: -250px;
        background-image: linear-gradient(to bottom, #eee956, #efef5d, #f0f463, #f0fa6a, #f1ff71);
        transform: rotate(45deg);
        z-index: 2;
    }
    .bar2{
        position:absolute;
        right: 300px;
        top: 28px;
        background-image: linear-gradient(to bottom, #d18585, #dc9a9a, #e7afae, #f1c4c4, #fbd9d9);       
        transform: rotate(45deg);
        z-index: 2;
    }
    .bar3{
        position: absolute;
        right: 0;
        top: 25px;
        background-image: linear-gradient(to bottom, #e8f9f9, #d2f5f6, #baf0f3, #a1ecf1, #84e7ef);
        transform: rotate(45deg);
        z-index: 2;
    }
    .bar4{
        position: absolute;
        right: 0;
        top: -633px;
        background-image: linear-gradient(to bottom, #d18585, #dc9a9a, #e7afae, #f1c4c4, #fbd9d9);
        transform: rotate(-45deg);
        z-index: 1;
    }
    .bar5{
        position: absolute;
        right: 0;
        top: -336px;
        background-image: linear-gradient(to bottom, #e8f9f9, #d2f5f6, #baf0f3, #a1ecf1, #84e7ef);
        transform: rotate(-45deg);
        z-index: 1;
    }

</style>
