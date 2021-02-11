<template>
    <div class="sidebar text-light position-fixed h-100 bg-info top-0 start-0">
        <FormCredidentials @update-creds="updateCreds($event)"/>
        <div class="text-center lead m-4">
            Sélectionnez une base de données
        </div>
        <ul class="list-group overflow-auto">
            <li v-for="database in response" v-bind:key="database" @click="selectedDB = database" class="list-group-item py-1" :class="selectedDB == database ? 'selected' : 'bg-info'">
                {{ database }}
            </li>
        </ul>
        <div class="sidebar fixed-bottom mb-2 bg-info">
            <div class="m-2 mb-3 h2 text-center" style="cursor: pointer" @click="getList()">
                <i class="bi bi-arrow-repeat"></i>
            </div>
            <div class="mx-2">
                <div class="mb-2">
                    <input type="text" v-model="nameNewDB" placeholder="Entrez un nom pour la nouvelle DB" class="w-100 text-center form-control"/>
                </div>
                <div class="d-flex justify-content-center">
                    <button class="btn btn-info btn-block" @click="addDB()"><i class="bi bi-plus-round-fill"></i> Ajouter</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import FormCredidentials from './FormCredidentials.vue'
    export default {
        name: 'DatabaseList',
        components: { FormCredidentials },

        data() {
            return {
                creds: {
                    username: 'admin',
                    password: 'admin',
                },

                response: null,
                selectedDB: null,
                nameNewDB: null,
            }
        },

        watch: {
            selectedDB: function() {
                this.$emit('changing-db', this.selectedDB);
            }
        },

        created() {
            this.getList();
        },

        methods: {
            getList: function() {
                this.$axios({
                    method: 'get',
                    url: 'http://127.0.0.1:5984/_all_dbs',
                    auth: this.creds,
                })
                .then(_ => {
                    this.response = _.data;
                })
                .catch(err => {
                    console.log(err);
                });
            },

            addDB: function() {
                if(this.nameNewDB) {
                    this.$axios({
                        method: 'put',
                        url: 'http://127.0.0.1:5984/'+this.nameNewDB,
                        auth: this.creds,
                    })
                    .then(_ => {
                        console.log(_);
                        this.nameNewDB = null;
                        this.getList();
                    })
                    .catch(err => {
                        console.log(err);
                    });
                }
            },

            updateCreds: function(e) {
                this.creds = e;
                this.$emit('update-creds', e);
            },
        }
    }

</script>

<style scoped>
    .sidebar {
        width: 300px;
    }

    .list-group {
        cursor: pointer;
    }

    .selected {
        color: #000 !important;
        background-color: #fff;
        font-weight: bold;
    }
</style>