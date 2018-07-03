<template>
    <section>
        <router-link to="/">
            <button class="arrowBack"><i class="fas fa-long-arrow-alt-left"></i> <span>Powrót</span> </button>
        </router-link>
        
        <div class="info"><h3>Proszę, uzupełnij pola poniżej:</h3> </div>
        <form action="">
            <input type="text" placeholder="Nazwa produktu" required v-model="name">
            <input type="number" placeholder="Ile kalorii na 100g?" required v-model="calories">
            <div class="row">
                <input type="number" placeholder="B" v-model="protein"> 
                <input type="number" placeholder="W" v-model="carb"> 
                <input type="number" placeholder="T" v-model="fat">
            </div>
            <button @click="add"><i class="fas fa-plus"></i></button>
        </form>
    </section>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
    name: 'add',
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
    -webkit-box-shadow: 5px 5px 5px -5px rgba(0,0,0,0.75);
    -moz-box-shadow: 5px 5px 5px -5px rgba(0,0,0,0.75);
    box-shadow: 5px 5px 5px -5px rgba(0,0,0,0.75);
    width: 300px;
        padding: 50px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
        margin-top: 70px;
        align-items: center;
        background-color: #7FDBFF;
    }
    h3{
        margin-bottom: 20px;
    }
    form{
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    input{
        padding: 6px;
        width: 200px;
        font-size: 16px;
        border: 0;
        border-radius: 5px;
        margin-bottom: 10px;
        -webkit-box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
        box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
    }
    
    .row {
        display: flex;
        margin-bottom: 15px;
    }
    .row input{
        width: 30%;
        margin: 0 auto;
    }

    button{
        cursor: pointer;
        margin-top: 5px;
        padding: 8px;
        width: 40px;
        height: 40px;
        background-color: #0074D9;
        border: none;
        border-radius: 50%;
        color: mintcream;
        -webkit-box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
        box-shadow: 0px 1px 1px 0px rgba(0,0,0,0.75);
        font-size: 20px;
    }

    input:focus, button:focus{
        outline: 0;
    }
    .arrowBack{
        -webkit-box-shadow: 2px 2px 2px -2px rgba(0,0,0,0.75);
        -moz-box-shadow: 2px 2px 2px -2px rgba(0,0,0,0.75);
        box-shadow: 2px 2px 2px -2px rgba(0,0,0,0.75);
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
        top: -50px;
        left: -80px;
        font-size: 30px;
        padding: 0;
        border-radius: 0;
        width: 120px;
    }
    .arrowBack span{
        font-size: 20px;
        padding-left: 10px;
    }
    a {
        text-decoration: none;
    }
</style>






