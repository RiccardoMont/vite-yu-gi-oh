<script>
import axios from 'axios';
import AppCardCatalogue from './AppCardCatalogue.vue';


export default {
    name: 'AppMain',
    data() {
        return {
            list: [],
            archetypes: [],
            selectedArchetype: null
        }
    },
    components: {
        AppCardCatalogue
    },
    mounted() {
        axios
            .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=50&offset=0')
            .then((response) => {
                console.log(response.data);
                this.list = response.data.data;
                console.log(this.list[0].archetype);
                
                for (let i = 0; i < this.list.length; i++) {
                    this.list[i].show = true;
                    console.log(this.list[i].show);
                    if (this.list[i].archetype) {
                        this.archetypes.push(this.list[i].archetype);
                    }
                }
            })
    },
    methods: {
        archetypesFilter(){
            console.log(this.selectedArchetype);
            const filtro = this.list.filter(singlecard => {
                if(this.selectedArchetype === 'All'){
                    singlecard.show = true;
                } else if (this.selectedArchetype !== singlecard.archetype){
                    singlecard.show = false;
                } else {
                    singlecard.show = true;
                }
            })
        }

    }
}
</script>
<template>
    <div class="containerSelect">
        <label for="DeckArchetype">Deck Archetype</label>
        <select @change="archetypesFilter()" v-model="selectedArchetype" name="deckArcs" id="DeckArchetype">
            <option value="All" selected>All</option>
            <option v-if="list != undefined" v-for="(singleArchetype, index) in new Set(archetypes)" :value="singleArchetype">{{ singleArchetype }}</option>
        </select>
    </div>
    <div class="container">
        <div class="row">
            <AppCardCatalogue v-if="list != undefined" v-for="(singlecard, index) in list" :singlecard="singlecard">
            </AppCardCatalogue>
        </div>
    </div>
</template>
<style scoped>
.containerSelect {
    width: 80%;
    max-width: 1176px;
    margin: auto;
    padding: 2rem 0;
}

.container {
    background-color: white;
    display: flex;
    justify-content: center;
}

.row {
    width: 90%;
    justify-content: space-between;
}
</style>