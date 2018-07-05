<template>
    <main>
        <side-menu />
        <section id="search">
            <div class="info">        
                <h1>Dodaj swoje produkty</h1>
            </div>
            <input type="text" placeholder="Wyszukaj..." v-model="searchInput" id="searchBar">
            <div class="searchContainer" :class="{visible: searchVisible}">                
                <div class="searchedItems" v-for="item in searched">
                    <div class="searchedSingleItem" @click="select(item)">
                        <p> <span>{{item.name}}</span></p>
                    </div>
                </div>
            <router-link to="/add">
                <div class="addItem">
                    <p> Nie znalazłeś? Dodaj produkt!</p>
                </div>
            </router-link>
            </div>
        </section>
        <section id="items" > 
            <div class="info">
                <h1> Wybrane </h1>
            </div>
            <div class="singleItem" v-for="item in selected" :key="item.id">
                <button class="close"  @click="select(item)" ><i class="fas fa-trash-alt"></i></button>
                <div class="singleItemInfo">
                    <div class="name"> {{item.name}} </div>
                    <div class="grams">
                        <input type="number" v-model="item.gramsEaten" @blur="calculateEatenCalories(item)" 
                        @change="calculateEatenCalories(item)" @input="calculateEatenCalories(item)">g
                    </div>
                </div>
            </div>
        </section>
        <section id="output"> 
            <div class="info">
                <h1>Wyniki</h1>
            </div>
            <div class="macros">
                <div class="kcal" >{{kcalCalculated}} kcal</div>
                <div class="chart">
                    <div class="macroItem">
                        <div class="macroBar">
                            <div id="fat" :style="{height: [fatHeight+'%']}"></div>
                        </div>
                        Tłuszcze
                        <span>{{fatCalculated}}g</span>
                    </div>
                    <div class="macroItem">
                        <div class="macroBar">
                            <div id="carb" :style="{height: [carbHeight+'%']}"></div>
                        </div>
                         Węglowod.
                        <span>{{carbCalculated}}g</span>
                    </div>
                    <div class="macroItem">
                        <div class="macroBar">
                            <div id="prot" :style="{height: [protHeight+'%']}"></div>
                        </div>
                        Białka
                        <span>{{proteinCalculated}}g</span>
                    </div>
       
                </div>
            </div>
        </section>
    </main>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
import SideMenu from './SideMenu'

export default {
    name: 'dashboard',
    components: {
        SideMenu,
    },
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
    
    
    a{
        text-decoration: none;
    }
    
    main{
        display: grid;
        grid-template-columns: 160px .5fr 1fr .5fr 1fr .5fr 1fr .5fr;
        background: url('../assets/dashboardbg.svg');
        background-repeat: no-repeat;
        background-position: right;
        grid-column-gap: 10px;
        grid-template-areas:
        'sidemenu . search . items . output .'
    }

    side-menu{
        grid-area: sidemenu;
    }
    #search{
        grid-area: search;
    }
    #items{
        grid-area: items;
    }
    #output{
        grid-area: output;
    }
    
    section{
        margin-top: 198px;
    }

    h1{
        font-size: 36px;
        margin-bottom: 63px;
        text-align: center;
    }

    input:focus{
        outline: 0;
    }

    #searchBar{
        width: 400px;
        height: 50px;
        font-size: 20px;
        border: 0;
        -webkit-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        -moz-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.16);
        padding: 0 25px;
    }

    .searchContainer{
        display: none;
        margin-top: 16px;
        -webkit-box-shadow: 0px 5px 6px 0px rgba(0,0,0,0.16);
        -moz-box-shadow: 0px 5px 6px 0px rgba(0,0,0,0.16);
        box-shadow: 0px 5px 6px 0px rgba(0,0,0,0.16);
    }
    .visible{
        display: block;
    }

    .searchedItems{
        background-color:rgba(249, 248, 248, 0.8);
        color: rgb(58, 58, 58);
        transition: 1s;
    }
    .searchedItems:nth-child(even){
        background-color: rgba(244, 240, 240, 0.8);
    }

    .searchedSingleItem{
        height: 47px;
        transition: .5s;
        cursor: pointer;
        display: flex;
        align-items: center;
        padding-left: 24px;
        font-size: 15px;
    }
    .searchedSingleItem:hover{
        color: #000;
        -webkit-box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.2);
        -moz-box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.2);
        box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.2);
    }
    .addItem{
        text-align: center;
        padding: 19px 0;
        color: #707070;
    }

    .singleItem{
        display: grid;
        width: 339px;
        height: 55px;
        grid-template-columns: 55px 1fr;
        margin-bottom: 3px;
        -webkit-box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
        -moz-box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
        box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
    }

    .singleItem button{
        background-color: #DFA0A0;
        border: 0;
        color: #fff;
        font-size: 30px;
    }

    .singleItemInfo{
        padding-left: 20px;
        background-color: #fff;
        color: #5D5C5C;
        display: grid;
        grid-template-columns: 70% 30%;
        font-size: 14px;
        text-overflow: clip;
        justify-content: center;
        align-content: center;
    }
    .grams{
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .singleItemInfo input{
        width:45px;
        border: 0;
        text-align: left;
        color: #5D5C5C;
        -webkit-box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
        -moz-box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
        box-shadow: 0px 1px 15px 0px rgba(0,0,0,0.08);
    }

    .macros{
        width: 312px;
        height: 363px;
        background-color: #fff;
        -webkit-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.25);
        -moz-box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.25);
        box-shadow: 0px 3px 6px 0px rgba(0,0,0,0.25);
    }

    .kcal{
        text-align: center;
        padding: 30px 0;
        font-size: 40px;
    }
    .chart{
        display: flex;
        justify-content: center;
    }

    .macroItem{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-left: 15px;
        margin-right: 15px;
    }

    .macroBar{
        display: flex;
        width: 52px;
        height: 185px;
        align-items: flex-end;
        font-size: 8px;
        margin-bottom: 10px;
    }

    #fat{
        display: flex;
        align-items: flex-end;
        justify-content: center;
        width: 100%;
        min-height: 5%;
        text-align:center;
        background-image: linear-gradient(to left bottom, #eee956, #efef5d, #f0f463, #f0fa6a, #f1ff71);
        transition: 1s;
    }

    #carb{ 
        display: flex;
        align-items: flex-end;
        justify-content: center;
        width: 100%;
        min-height: 7%;
        text-align:center;
        background-image: linear-gradient(to left bottom, #e8f9f9, #d2f5f6, #baf0f3, #a1ecf1, #84e7ef);
        transition: 1s;
    }

    #prot{
        display: flex;
        align-items: flex-end;
        justify-content: center;
        width: 100%;
        min-height: 7%;
        align-content: flex-end;
        text-align:center;
        color: #fff;
        background-image: linear-gradient(to left bottom, #fbd9d9, #f1c4c4, #e7afae, #dc9a9a, #d18585);
        transition: 1s;
    }

    @media screen and (max-width: 1000px){
        main{
            min-height: 100vh;
            display: flex;
            flex-direction:column;
            background: url('../assets/dashboardbgmobile.svg');
            background-repeat: no-repeat;
            background-size: cover;
        }
        h1{
            font-size: 25px;
            margin-bottom: 25px;
        }

        #search{
            margin: 0 auto;
            margin-top: 60px;
        }
        
        #items{
            margin: 0 auto;
            margin-top: 30px;
        }

        #output{
            margin: 0 auto;
            margin-top: 30px;
            margin-bottom: 30px;
        }

        #searchBar{
            width: 90vw;
        }
        .searchContainer{
            width: 90vw;
        }
        .searchedItems{
            width: 90vw;
        }
        .searchedSingleItem{
            width: 90vw;
        }

        .singleItem{
            width: 90vw;
        }
    }

    @media screen and (max-width: 1600px) and (min-width: 1000px){
        main{
            display: grid;
            min-height: 100vh;
            grid-template-columns: 120px .5fr 1fr .5fr 1fr .5fr 1fr .5fr;
            background: url('../assets/dashboardbg.svg');
            background-repeat: no-repeat;
            background-position: right;
            background-size: cover;
            grid-column-gap: 10px;
            grid-template-areas:
            'sidemenu . search . items . output .'
        }
        h1{
            font-size: 30px;
        }

        #search h1{
            margin-bottom: 28px;
        }
         #searchBar{
            width: 300px;
        }
        .searchContainer{
            width: 300px;
        }
        .searchedItems{
            width: 300px;
        }
        .singleItem{
            width: 300px;
        }

        .macros{
            width: 250px;
        }
        .macroItem{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-left: 8px;
        margin-right: 8px;
    }
    }
</style>
