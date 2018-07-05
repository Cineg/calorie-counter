<template>
    <section>
        <side-menu />
        <div class="wrapper">
            <h1>Dodaj nowy produkt</h1>
            <form action="">
                <div class="row"> 
                    <input type="text" placeholder="Nazwa produktu" required v-model="name" class="name">
                     <input type="number" placeholder="Ile kalorii na 100g?" required v-model="calories" class="calories">
                </div>
                <div class="row rowMacros">
                    <input type="number" placeholder="Białko" v-model="protein"> 
                    <input type="number" placeholder="Węglowodany" v-model="carb"> 
                    <input type="number" placeholder="Tłuszcze" v-model="fat">
                </div>
                <button @click="add">Dodaj</button>
            </form>
        </div>
    </section>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
import SideMenu from './SideMenu'

export default {
    name: 'add',
    components: {
        SideMenu,
    },
    data: function(){
        return{
            name: '',
            calories: '',
            fat: '',
            carb: '',
            protein: '',
        }
    },
    methods:{
        add: function(){ 
            if(this.name.trim() == '' || this.calories == ''){
                alert("Proszę, wypełnij podane pola");
            } else {
                let data = {
                    name: this.name.toLowerCase(),
                    calories: this.calories,
                    grams: 100,
                    gramsEaten: 100,
                    caloriesEaten: this.calories,
                    fat: this.fat,
                    carb: this.carb,
                    protein: this.protein,
                }
                    if(data.fat == ''){ data.fat = 0};
                    if(data.carb == ''){ data.carb = 0};
                    if(data.protein == ''){ data.protein = 0};
                db.collection('food').add(data).then(this.$router.push('/'));
            }
        }
    }
}
</script>

<style scoped>

    section{
        width: 100vw;
        min-height: 100vh;
        background: url('../assets/dashboardbg.svg');
        background-size: cover;
        background-repeat: no-repeat;
        background-position: top right;
        display: grid;
        grid-template-columns: 160px 1fr;
        grid-template-areas: 
        'sidemenu newitem '
    }

    side-menu{
        grid-area: sidemenu;
    }
    .wrapper{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        grid-area: newitem;
    }

    h1{
        font-size: 55px;
        margin-bottom: 40px;
    }
    form{
        background-color: #fff;
        width: 60vw;
        max-width: 950px;
        max-height: 450px;
        height: 50vh;
        display: flex;
        flex-direction: column;
        -webkit-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
        -moz-box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
        box-shadow: 0px 3px 8px 0px rgba(0,0,0,0.18);
    }

    input{
        background-color: #FCFCFC;
        border: 0;
        -webkit-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        -moz-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
    }
    .row{
        display: flex;
        margin-top: 50px;
        justify-content: center;
    }

    .name{
        height: 75px;
        font-size: 25px;
        padding: 20px;
        width: 60%;
        align-self: center;
        margin-right: 20px;
    }

    .calories{
        height: 75px;
        font-size: 15px;
        padding-left: 20px;
        width: 30%;
        align-self: center;
    }

    .rowMacros input{
        width: 20%;
        max-width: 210px;
        margin-right: 3%;
        height: 60px;
        padding-left: 20px;
    }

    button{
        margin: 0 auto;
        margin-top: 50px;
        border: 0;
        background: #D7F7A5;
        height: 80px;
        width: 60%;
        font-size: 38px;
        -webkit-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        -moz-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
    }

    @media screen and (max-width: 1000px){

        section{
            display: flex;
            flex-direction: column;
            background: url('../assets/dashboardbgmobile.svg');
            background-repeat: no-repeat;
            background-size: cover;
        }
        
        h1{
            margin-top: 100px;
            font-size: 40px;
            text-align: center;
        }

        form{
            width: 90vw;
            margin-bottom: 25px;
            height: inherit;
            padding-bottom: 30px;
        }

        .row{
            margin-top: 25px;
        }
        .name{
            font-size: 18px;
            height: 60px;
            margin-right: 10px;
        }

        .calories{
            height: 60px;
            width: 33%;
            font-size: 10px;
            padding-left: 5px;
        }

        .rowMacros input{
            width: 28%;
            margin-right: 3%;
            height: 40px;
            font-size: 12px;
            padding-left: 5px;
        }

        button{
            margin-top: 30px;
            font-size: 25px;
            height: 60px;
        }

    }

</style>






