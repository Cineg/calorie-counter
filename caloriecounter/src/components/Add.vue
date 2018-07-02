<template>
    <section>
        <div class="info"><h3>Proszę, uzupełnij pola poniżej:</h3> </div>
        <form action="">
            <input type="text" placeholder="Nazwa produktu" required v-model="name">
            <input type="number" placeholder="Ile kalorii na 100g?" required v-model="calories">
            <button @click="add">Dodaj</button>
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
            calories: ''
        }
    },
    methods:{
        add: function(){ 
            let data = {
                name: this.name.toLowerCase(),
                calories: this.calories,
                grams: 100,
                gramsEaten: 100,
                caloriesEaten: this.calories,
            }
            db.collection('food').add(data).then(this.$router.push('/'));
        }
    }
}
</script>

<style scoped>
    section{
        width: 600px;
        padding: 50px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
        margin-top: 70px;
        align-items: center;
        background-color: mistyrose;
        border-radius: 15px;
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
        width: 300px;
        font-size: 16px;
        border: 1px solid lightslategray;
        border-radius: 5px;
        margin-bottom: 5px;
    }

    button{
        margin-top: 5px;
        padding: 8px;
        width: 60px;
        background-color: lightskyblue;
        border: none;
        border-radius: 10px;
    }

    input:focus, button:focus{
        outline: 0;
    }
</style>






