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
            .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=100&offset=0')
            .then((response) => {

                this.list = response.data.data;

                for (let i = 0; i < this.list.length; i++) {

                    this.list[i].show = true;

                    if (this.list[i].archetype) {
                        this.archetypes.push(this.list[i].archetype);
                        this.archetypes.sort();
                    }
                }
            })
    },
    methods: {
        archetypesFilter() {
            console.log(this.selectedArchetype);
            const filtro = this.list.filter(singlecard => {
                if (this.selectedArchetype === 'All') {
                    singlecard.show = true;
                } else if (this.selectedArchetype !== singlecard.archetype) {
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
    <div class="container">
        <div class="containerSelect">
            <label for="DeckArchetype">Deck Archetype</label>
            <select @change="archetypesFilter()" v-model="selectedArchetype" name="deckArcs" id="DeckArchetype">
                <option value="All" selected>All</option>
                <option v-if="list != undefined" v-for="(singleArchetype, index) in new Set(archetypes)"
                    :value="singleArchetype">{{ singleArchetype }}</option>
            </select>
        </div>
        <div class="containerCatalogue">
            <div class="row">
                <AppCardCatalogue v-if="list != undefined" v-for="(singlecard, index) in list" :singlecard="singlecard">
                </AppCardCatalogue>
            </div>
        </div>
    </div>
</template>
<style scoped>
.containerSelect {
    padding: 2rem 0;
}

.containerCatalogue {
    background-color: white;
    display: flex;
    justify-content: center;
    margin-bottom: 6rem;
}

.row {
    width: 100%;
    padding: 3rem;
}
</style>