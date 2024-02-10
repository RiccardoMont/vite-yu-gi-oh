<script>
import axios from 'axios';
import AppCardCatalogue from './AppCardCatalogue.vue';


export default {
    name: 'AppMain',
    data() {
        return {
            //Array dove poter inserire la lista di carte data dal response
            list: [],

            //Array d'appoggio per creare dinamicamente le voci della select
            archetypes: [],

            //Parametro per prendere il value del select durante l'utilizzo del filtro
            selectedArchetype: null,

            //Parametro per il v-show della barra dei risultati
            results: false,

            //Parametro per il conteggio del numero delle carte trovate dopo la ricerca effettuata
            count: 0,

            //V-show per il loader
            loading: true

            
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
                //Tolgo il loader una volta fatta la chiamata
                this.loading = false;

                //Ciclo per la creazione delle options nella select in modo dinamico
                for (let i = 0; i < this.list.length; i++) {

                    //Aggiunto parametro per il v-show ad ogni oggetto-carta
                    this.list[i].show = true;

                    //Push dell'archetipo nell'array di supporto 
                    if (this.list[i].archetype) {
                        this.archetypes.push(this.list[i].archetype);

                        //Organizzo le options in ordine alfabetico
                        this.archetypes.sort();
                    }
                }
            })
    },
    methods: {
        //Funzione della select
        archetypesFilter() {

            //Reimposto il contatore a 0 per evitare che si sommi con la ricerca precedente
            this.count = 0;

            //Filtro in azione che setta i parametri v-show del div-results e delle cards oltre che incrementare il parametro di conteggio dei risultati
            const filtro = this.list.filter(singlecard => {
                if (this.selectedArchetype === 'All') {
                    singlecard.show = true;
                    this.results = false;
                } else if (this.selectedArchetype !== singlecard.archetype) {
                    singlecard.show = false;
                } else {
                    singlecard.show = true;
                    this.results = true;
                    this.count++;
                }
            })
        }

    }
}
</script>
<template>
    <div class="container">
        <div class="containerSelect">
            <label for="DeckArchetype">Deck Archetype:</label>
            <select @change="archetypesFilter()" v-model="selectedArchetype" name="deckArcs" id="DeckArchetype">
                <option value="All" selected>All</option>
                <option v-if="list != undefined" v-for="(singleArchetype, index) in new Set(archetypes)"
                    :value="singleArchetype">{{ singleArchetype }}</option>
            </select>
        </div>
        <div class="containerCatalogue" >
            <div v-show="this.results" class="results">
                <span>Cards found: {{ this.count }}</span>
            </div>
            <div v-if="!loading" class="row">
                <AppCardCatalogue v-if="list != undefined" v-for="(singlecard, index) in list" :singlecard="singlecard">
                </AppCardCatalogue>
            </div>
            <div v-else class="loader">
                <span>Loading...</span>
            </div>
        </div>
    </div>
</template>
<style scoped>
.containerSelect {
    padding: 2rem 0;

    & label {
        font-size: 20px;
        font-weight: 700;
    }

    & select {
        margin-left: 1rem;
        padding: 0.5rem;
        border-radius: 18px;
    }

    & select:active{
        border-collapse: collapse ;
        
    }
}

.containerCatalogue {
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: 6rem;
    border-radius: 1rem;

    .loader {
        & span{
            font-size: 30px;
            font-weight: 700;
            animation: gradient 6s ease infinite;
        }
    }
}

.results {
    width: calc(100% - 8rem);
    background-color: black;
    padding: 1rem 0.5rem;

    & span {
        color: white;
        font-weight: 700;
    }
}

.row {
    width: 100%;
    padding: 1rem 3rem 3rem 3rem;
}


@keyframes gradient {
	0% {
		color: black;
	}
    20% {
        color: blue;
    }
	40% {
		color: red;
	}
	60% {
		color: darkorange;
	}
    80% {
        color: green;
    }
    100% {
        color: black;
    }
}

</style>