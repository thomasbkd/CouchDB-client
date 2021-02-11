<template>
    <div class="sidebar text-light position-fixed h-100 bg-info top-0 start-0">
        <div class="text-center lead m-4">
            Sélectionnez une base de données
        </div>
        <ul class="list-group">
            <li v-for="database in response" v-bind:key="database" @click="selectedDB = database" class="list-group-item" :class="selectedDB == database ? 'selected' : 'bg-info'">
                {{ database }}
            </li>
        </ul>
        <div class="sidebar fixed-bottom mb-2">
            <div class="mx-2">
                <div class="mb-2">
                    <input type="text" v-model="nameNewDB" placeholder="Entrez un nom pour la nouvelle DB" class="w-100 text-center"/>
                </div>
                <div class="d-flex justify-content-center">
                    <button class="btn btn-info btn-block" @click="addDB()">Ajouter</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'DatabaseList',

        data() {
            return {
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
                    auth: {
                        username: 'admin',
                        password: 'admin',
                    }
                })
                .then(_ => {
                    this.response = _.data;
                })
                .catch(err => {
                    console.log(err);
                });
            },

            addDB: function() {
                this.$axios({
                    method: 'put',
                    url: 'http://127.0.0.1:5984/'+this.nameNewDB,
                    auth: {
                        username: 'admin',
                        password: 'admin',
                    }
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