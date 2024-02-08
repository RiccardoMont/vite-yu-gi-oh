<script>
import axios from 'axios';
import AppSelectOptions from './AppSelectOptions.vue';
import AppCardCatalogue from './AppCardCatalogue.vue';


export default {
    name: 'AppMain',
    data() {
        return {
            list: [],
            archetypes: []
        }
    },
    components: {
        AppSelectOptions,
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
                    if (this.list[i].archetype) {
                        this.archetypes.push(this.list[i].archetype);
                    }
                }
            })
    }
}
</script>
<template>
    <div class="containerSelect">
        <label for="DeckArchetype">Deck Archetype</label>
        <select name="deckArcs" id="DeckArchetype">
            <AppSelectOptions v-if="list != undefined" v-for="(singleArchetype, index) in new Set(archetypes)"
                :singleArchetype="singleArchetype"></AppSelectOptions>
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
    justify-content: center;
}
</style>