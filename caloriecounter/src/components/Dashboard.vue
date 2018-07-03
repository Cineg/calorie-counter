<template>
    <main>
        <section id="search">
            <div class="info">        
                <h4>Wyszukaj i wybierz produkty, aby wyliczyć kalorie! <br />
                <span>
                    <i class="fas fa-angle-double-down"></i>
                    <i class="fas fa-balance-scale"></i> 
                </span>
                </h4>
            </div>
            <div class="search-bar">
                <input type="text" placeholder="Wyszukaj..." v-model="searchInput">
                <button class="searchButton">
                    <i class="fas fa-search"></i>
                </button>
            </div>
            <div class="searchContainer" :class="{visible: searchVisible}">                
                <div class="searchedItems" v-for="item in searched">
                    <div class="searchedSingleItem" @click="select(item)">
                        <p> <span>{{item.name}}</span>: {{item.calories}} kcal na {{item.grams}}g</p>
                    </div>
                </div>
            <router-link to="/add">
                <div class="addItem">
                    <p> Nie ma tego, czego szukasz? Dodaj produkt. <i class="fas fa-shopping-cart"></i></i></p>
                </div>
            </router-link>
            </div>
        </section>
        <section id="items" > 
            <div class="info">
                <h4>Twoje wybrane produkty! <br />
                <span>
                <i class="fas fa-angle-double-down"></i>
                <i class="fas fa-clipboard-list"></i>
                </span>
                </h4>
            </div>
            <div class="singleItem" v-for="item in selected" :key="item.id">
                <div class="close"  @click="select(item)" ><i class="fas fa-trash-alt"></i></div>
                <div class="name"> {{item.name}} </div>
                <div class="grams">
                    <input type="number" v-model="item.gramsEaten" @blur="calculateEatenCalories(item)" 
                    @change="calculateEatenCalories(item)" @input="calculateEatenCalories(item)">g
                </div>
                <div class="calories">{{item.caloriesEaten}} kcal</div>
            </div>
        </section>
        <section id="output"> 
            <div class="info">
                <h4>Wyliczone kalorie oraz składniki! <br />
                <span>
                <i class="fas fa-angle-double-down"></i>
                <i class="fas fa-burn"></i>
                </span>
                </h4>
            </div>
            <div class="macros">
                <div >
                    <div id="fat" :style="{height: [fatHeight+'%']}">
                        Tłuszcz: {{fatCalculated}}g
                    </div>
                </div>
                <div >
                    <div id="carb" :style="{height: [carbHeight+'%']}">
                        Węglowodany: {{carbCalculated}}g
                        </div>
                </div>
                <div>
                    <div id="prot" :style="{height: [protHeight+'%']}">
                        Białko: {{proteinCalculated}}g
                    </div>
                </div>
            </div>
            <div class="kcal" v-if="kcalCalculated">{{kcalCalculated}} kcal</div>
        </section>
    </main>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'

export default {
    name: 'dashboard',
    data(){
        return{
            searchInput: '',
            searchVisible: false,
            nothingFound: true,
            kcalCalculated: 0,
            fatCalculated: 0,
            carbCalculated: 0,
            proteinCalculated: 0,
            allMacros: 0,
            fatHeight: 0,
            carbHeight: 0,
            protHeight: 0,
            items: []
        }
    },
    created(){
        db.collection('food').get().then(querySnapshot => {
            querySnapshot.forEach(doc => {
                const data = {
                    'id': doc.id,
                    'name': doc.data().name,
                    'calories': doc.data().calories,
                    'grams': doc.data().grams,
                    'gramsEaten': doc.data().grams,
                    'caloriesEaten': doc.data().calories,
                    'fat': doc.data().fat,
                    'fatEaten': doc.data().fat,
                    'carb': doc.data().carb,
                    'carbEaten': doc.data().carb,
                    'protein': doc.data().protein,
                    'proteinEaten': doc.data().protein,
                    'selected': false,     
                }
                this.items.push(data);
            })
        })
    },
    methods:{
        select: function(item){
            if(!item.selected){
                item.selected = true;
                this.calcAll();
            }else{
                item.selected = false;
                this.calcAll();
            }
        },
        calculateEatenCalories: function(item){

            //calculate how many calories etc eaten in each item
            let kcal = item.calories * (item.gramsEaten/item.grams);
            let fats = item.fat * (item.gramsEaten/item.grams);
            let carbs = item.carb * (item.gramsEaten/item.grams);
            let proteins = item.protein * (item.gramsEaten/item.grams);            
            item.caloriesEaten = kcal.toFixed(2);
            item.caloriesEaten = parseFloat(item.caloriesEaten);
            item.fatEaten = fats.toFixed(2);
            item.fatEaten = parseFloat(item.fatEaten);
            item.carbEaten= carbs.toFixed(2);
            item.carbEaten = parseFloat(item.carbEaten);
            item.proteinEaten= proteins.toFixed(2);
            item.proteinEaten = parseFloat(item.proteinEaten);

            this.calcAll();
        },
        calcAll: function(){
            //get all items added
            let items = this.items.filter(item => item.selected == true);

            //sum items values
            let kcal = items.reduce((sum, item) => {
                return sum + parseFloat(item.caloriesEaten);
            }, 0)
            let fats = items.reduce((sum, item) => {
                return sum + parseFloat(item.fatEaten);
            }, 0)
            let carbs = items.reduce((sum, item) => {
                return sum + parseFloat(item.carbEaten);
            }, 0)
            let proteins = items.reduce((sum, item) => {
                return sum + parseFloat(item.proteinEaten);
            }, 0)
            
            //round to two places after decimal
            this.kcalCalculated = Math.round(kcal * 100) / 100;
            this.fatCalculated = Math.round(fats * 100) / 100;
            this.carbCalculated = Math.round(carbs * 100) / 100;
            this.proteinCalculated = Math.round(proteins * 100) / 100;
            
            this.computedBars();
            
        },
    computedBars: function(){
        this.allMacros = parseFloat(this.fatCalculated)+parseFloat(this.carbCalculated)+parseFloat(this.proteinCalculated);

        let percent = 100 / this.allMacros;
        let percentRoundFat = Math.round(this.fatCalculated * percent);
        let percentRoundCarb = Math.round(this.carbCalculated * percent);
        let percentRoundProt = Math.round(this.proteinCalculated * percent)
        
        this.fatHeight = percentRoundFat;
        this.carbHeight = percentRoundCarb ;
        this.protHeight = percentRoundProt;
    }

    },
    computed:{
        //search engine
        searched: function(){
            let search = this.searchInput.toLowerCase();
            if(this.searchInput == ''){
                this.searchVisible = false;
                return
            } else {
                this.nothingFound = false;
                if(this.searchInput.length >= 3){
                    this.searchVisible = true;
                return this.items.filter(item => item.name.match(search)); 
                }
            }
        },
        selected: function(){
            return this.items.filter(item => item.selected == true);
        },
    }
}
</script>

<style scoped>
    
    
    main{
        display: grid;
        margin-top: 60px;
        grid-template-columns: repeat(12, 1fr);
        grid-column-gap: 5px;
    }
    #search{
        grid-area: search;
        text-align: center;
        margin-bottom: 30px;
    }

    #items{
        grid-area: items;
        text-align: center;
    }
    #output{
        grid-area: output;
        text-align: center;
    }


    .search-bar{
        display: flex;
        justify-content: center;
    }

    .search-bar input{
        padding: 15px;
        border: 0;
        border-top: 1px solid lightgray;
        border-left: 1px solid lightgray;
        border-bottom: 1px solid lightgray;
        border-radius: 10px 0px 0px 10px;
    }
    .search-bar button{
        padding: 15px;
        border: 0px;
        margin: 0;
        background-color: dimgray;
        color: white;
        border-radius: 0px 10px 10px 0px;
        cursor: pointer;
    }

    .search-bar input:focus, .search-bar button:focus, .grams input:focus{
        outline: none;
    }

    .searchContainer{
        margin: 0 auto;
        margin-top: 5px;
        border: 1px solid lavender;
        border-radius: 10px;
        display: none;
    }
    .visible{
        display: flex;
        flex-direction:column;
    }

    .searchedItems{
        transition: background-color 1s ease;
    }

    .searchedItems:nth-child(even){
        background-color: #f5f5f5;
    }
    .searchedItems:last-child{
        border-radius: 0px 0px 10px 10px;
    }

    .searchedSingleItem{
        display: flex;
        justify-content: center;
        cursor: pointer;
    }
    .searchedItems:hover{
        display: flex;
        justify-content: center;
        cursor: pointer;
        background-color: rgba(122, 189, 206, 0.2);
    }
    .searchedItems:nth-child(even):hover{
        background-color: rgba(122, 189, 206, 0.2);
    }

    .searchedSingleItem span{
        font-weight: 700;
    }

    .grams input{
        padding: 3px;
        border: 0;
        border-radius: 5px;
        width: 65px;
        -webkit-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        -moz-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
    }

    a{
        text-decoration: none;
        color: #2c3e50;
    }
    .addItem{
        padding: 10px;
        background-color: #2c3e50;
        border-radius: 10px;
        color: #ededed;
    }

    .singleItem{
        -webkit-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        -moz-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
    }

    .info{
        display: flex;
        flex-direction: column;
        margin: 0 auto;
        margin-bottom: 15px;
        width: 300px;
        height: 90px;
        padding: 15px;
        -webkit-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        -moz-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        border-radius: 50px 50px 50px 50px;
        align-content: center;
        justify-content: center;
        margin-bottom: 15px;
    }
    .info span{
        font-size: 120%;
    }

    .kcal{
        margin: 0 auto;
        padding: 30px;
        width: 150px;
        transition: 1s;
        -webkit-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        -moz-box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.2);
        margin-top: 15px;
    }

    .macros{
        display: flex;
        justify-content: center;
    }
    .macros >div{
        background-color: #fcfcfc;
        margin: 3px;
        height: 200px;
        width: 65px;
        display: flex;
        align-items: flex-end;
        font-size: 8px;
        -webkit-box-shadow: 5px 0px 5px -3px rgba(0,0,0,0.75);
        -moz-box-shadow: 5px 0px 5px -3px rgba(0,0,0,0.75);
        box-shadow: 5px 0px 5px -3px rgba(0,0,0,0.75);
    }

    #fat{
        justify-content: center;
        background-color: lemonchiffon;
        width:65px;
        min-height: 10px;
        transition: 2s;
    }
    #carb{
        background-color: lightsalmon;
        justify-content: center;
        width: 65px;
        min-height: 10px;
         transition: 2s;
    }
    #prot{
        background-color: powderblue;
        justify-content: center;
        width: 65px;
        min-height: 10px;
        transition: 2s;
    }

    /* Phones */
    @media screen and (max-width: 600px){
        main{
            grid-template-areas:
            "search search search search search search search search search search search search" 
            "items items items items items items items items items items items items" 
            "output output output output output output output output output output output output";
        }

        .searchedItems{
            margin: 0 auto;
            width: 100%;
            padding: 15px;
        }

        .search-bar input{
            width: 70%;
        }

        .singleItem{
            display: grid;
            width: 77%;
            grid-template-columns: 15% 25% 40% 20%;
            justify-content: center;
            align-items: center;
            margin: 0 auto;
            border-radius: 10px;
            margin-bottom: 10px;
            
        }

        .singleItem .close{
            padding: 20px 10px;
            border-radius: 10px;
            background-color: pink;
        }
        .searchContainer{
            width: 77%;
        }
    }
    /* Tablets and small laptops */
    @media screen and (min-width: 600px){
        main{
            grid-template-areas:
            "search search search search search search search search search search search search" 
            "items items items items items items output output output output output output" ;
        }
        .search-bar input{
            width: 60%;
        }
        .searchContainer{
            width: 65%;
        }
        .searchedSingleItem{
            margin: 0 auto;
            margin-bottom: 5px;
            padding: 10px;
        }
        .singleItem{
            display: grid;
            width: 77%;
            grid-template-columns: 15% 25% 40% 20%;
            justify-content: center;
            align-items: center;
            margin: 0 auto;
            border-radius: 10px;
            margin-bottom: 10px;
        }

        .singleItem .close{
            padding: 20px 10px;
            border-radius: 10px;
            background-color: pink;
        }
    }
    /* PC */
    @media screen and (min-width: 1000px){
        main{
        grid-template-areas:
        "search search search search items items items items output output output output" ;
        }
        .search-bar input{
            width: 80%;
        }
        .searchContainer{
            width: 85%;
        }
        .singleItem{
            display: grid;
            width: 77%;
            grid-template-columns: 20% 40% 20% 20%;
            justify-content: center;
            align-items: center;
            margin: 0 auto;
            border-radius: 10px;
            margin-bottom: 10px;
        }

        .singleItem .close{
            padding: 20px 10px;
            border-radius: 10px;
            background-color: pink;
        }
    }


</style>
