<template>
    <div class="container">
 
        <add-match @match-added="refresh"></add-match>
        <!-- Button trigger modal -->
        
            <ul class="list-group mt-3">
                <li class="list-group-item d-flex justify-content-between align-items-center" v-for="match in matches.data" :key="match.id">
                    <a> {{ match.equipe_a.name }} </a>
                    <p style="margin-bottom: auto; margin-top: auto;">contre</p>
                    <a> {{ match.equipe_b.name }} </a>
                        <a href="#"></a>
                        <div>
                        <button type="button" class="btn btn-secondary" data-toggle="modal" data-target="#editmatchModal" @click="getMatch(match.id)">
                            Editer
                        </button>
                        <button type="button" class="btn btn-danger" data-toggle="modal" @click="deleteMatch(match.id)">
                            Supprimer
                        </button>
                        </div>
                    </li>
                    <edit-match v-if="matchToEdit.id" v-bind:matchToEdit="matchToEdit" v-on:match-updated="refresh"></edit-match>
                </ul>
               <pagination :data="matches" @pagination-change-page="getResults" class="mt-5"></pagination>
    </div>
</template>

<script>
    export default {

        data() {
            return {
                matches: {},
                matchToEdit: {
                    id: '',
                    id_equipeA: '',
                    id_equipeB: ''
                },
                q : ''
            }
        },

        created() {
            axios.get('http://127.0.0.1:8000/api/matchesList')
                .then(response => this.matches = response.data.matches)
                .catch(error => console.log(error));
        },

        methods: {
            getResults(page = 1) {
			axios.get('http://127.0.0.1:8000/api/matchesList?page=' + page)
				.then(response => {
					this.matches = response.data.matches;
				});
        },

            getMatch(id){
                axios.get('http://127.0.0.1:8000/api/matches/edit/' + id)
                .then(response => this.matchToEdit = response.data)
                .catch(error => console.log(error));
            },

            deleteMatch(id){
                axios.delete('http://127.0.0.1:8000/api/matches/' + id)
                .then(response => this.matches = response.data.matches)
                .catch(error => console.log(error));
            },

        refresh(matches) {
            this.matches = matches.data.matches;
        }
        
        },


        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
