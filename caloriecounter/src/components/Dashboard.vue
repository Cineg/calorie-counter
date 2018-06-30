<template>
    <main>
        <section id="search">
            <div class="search-bar">
                <input type="text" placeholder="Wyszukaj..." v-model="searchInput">
                <button class="searchButton">
                    <i class="fas fa-search"></i>
                </button>
            </div>
            <div class="searchContainer" :class="{visible: searchVisible}">                
                <div class="searchedItems" v-for="item in searched">
                    <div class="searchedSingleItem" @click="select(item)">
                        <p> <span>{{item.name}}</span>: {{item.calories}} calories per {{item.grams}}g</p>
                    </div>
                </div>
            </div>
        </section>
        <section id="items" > 
            <div class="singleItem" v-for="item in selected" :key="item.id">
                <div class="name"> {{item.name}} </div>
                <div class="grams">
                    <input type="number" v-model="item.grams"> grams
                </div>
                <div class="calories">{{item.calories}} calories</div>
            </div>
        </section>
        <section id="output"> Output will be here</section>
    </main>
</template>

<script>
export default {
    name: 'dashboard',
    data(){
        return{
            searchInput: '',
            searchVisible: false,
            nothingFound: true,
            items: [
                {
                    id: 1,
                    name: 'pierś z kurczaka',
                    grams: 100,
                    calories: 239,
                    selected: false,
                },
                {
                    id: 2,
                    name: 'stek',
                    grams: 100,
                    calories: 271,
                    selected: false,   
                },
                {
                    id: 3,
                    name: 'woda',
                    grams: 100,
                    calories: 0,
                    selected: false,  
                },
                {
                    id: 4,
                    name: 'kawa, espresso',
                    grams: 100,
                    calories: 9,
                    selected: false,  
                },
            ]
        }
    },
    methods:{
        select: function(item){
            if(!item.selected){
                item.selected = true;
            }else{
                item.selected = false;
            }
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
                this.searchVisible = true;
                return this.items.filter(item => item.name.match(search)); 
            }
        },
        selected: function(){
            return this.items.filter(item => item.selected == true);
        },
    }
}
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Roboto');
    
    main{
        display: grid;
        margin-top: 60px;
        grid-template-columns: repeat(12, 1fr);
        grid-column-gap: 5px;
        font-family: 'Roboto', sans-serif;
        color: #2c3e50;
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

    .search-bar input:focus, .search-bar button:focus{
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

    .searchedItems:nth-child(even){
        background-color: #f5f5f5;
    }
    .searchedItems:last-child{
        border-radius: 0px 0px 10px 10px;
    }

    .searchedSingleItem{
        display: flex;
        justify-content: center;
    }
    .searchedSingleItem span{
        font-weight: 700;
    }

    .singleItem{
        padding: 20px;
        border-radius: 10px;
        background-color: #bae1ff;
        margin-bottom: 3px;
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
            margin-top: 4px;
            padding: 10px;
            width: 60%;
        }

        .search-bar input{
            width: 70%;
        }

        .singleItem{
            margin: 0 auto;
            margin-bottom: 10px;
            width: 90%;
        }
        .searchContainer{
            width: 78%;
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
    }


</style>
