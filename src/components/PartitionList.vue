<template>
    <div id="db-body">
        <div class="text-center lead pt-2">
            Base de données sélectionnée : 
            <span class="font-weight-bold">{{ db ? db : 'aucune' }}</span>
            <span v-if="db" class="ml-2"><button class="btn btn-sm btn-outline-danger" @click="deleteDB()"><i class="bi bi-trash-fill"></i> Supprimer la DB</button></span>
        </div>

        <div class="mt-3 p-4 text-left">
            <h2>Contenu de la base de données</h2>

            <code>{{ response }}</code>
        </div>
        
    </div>
</template>

<script>
    export default {
        name: 'DatabaseList',

        props: {
            db: {
                type: String,
            },

            creds: {
                type: Object,
                required: false,
            }
        },

        data() {
            return {
                response: null,
            }
        },

        watch: {
            db: function() {
                if(this.db) {
                    this.getList();
                }
            }
        },

        methods: {
            getList: function() {
                this.$axios({
                    method: 'get',
                    url: 'http://127.0.0.1:5984/'+this.db,
                    auth: this.creds,
                })
                .then(_ => {
                    this.response = _.data;
                })
                .catch(err => {
                    console.log(err);
                });
            },

            deleteDB: function() {
                if(window.confirm('Êtes-vous sûr de vouloir supprimer cette base de données ?')) {
                    this.$axios({
                        method: 'delete',
                        url: 'http://127.0.0.1:5984/'+this.db,
                        auth: this.creds,
                    })
                    .then(_ => {
                        console.log(_);
                        this.db = null;
                        this.response = null;
                    })
                    .catch(err => {
                        console.log(err);
                    });
                }
            }
        }
    }

</script>

<style scoped>
    #db-body {
        margin-left: 300px;
    }
</style>