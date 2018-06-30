<template>
    <main>
        <section id="search">
            <div class="search-bar">
                <input type="text" placeholder="Search..." >
                <button class="searchButton">
                    <i class="fas fa-search"></i>
                </button>
            </div>
        </section>
        <section id="items" > 
            <div class="singleItem" v-for="item in items" :key="item.id">
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
            items: [
                {
                    id: 1,
                    name: 'chicken',
                    grams: 100,
                    calories: 239,
                },
                {
                    id: 2,
                    name: 'steak',
                    grams: 100,
                    calories: 271   
                }
            ]
        }
    },
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
        .search-bar input{
            width: 70%;
        }

        .singleItem{
            margin: 0 auto;
            margin-bottom: 10px;
            width: 90%;
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
    }


</style>
