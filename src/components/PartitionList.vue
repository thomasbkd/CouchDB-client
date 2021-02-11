<template>
    <div id="db-body">
        <div class="text-center lead pt-2">
            Base de données sélectionnée : 
            <span class="font-weight-bold">{{ db ? db : 'aucune' }}</span>
        </div>
        
    </div>
</template>

<script>
    export default {
        name: 'DatabaseList',

        props: {
            db: {
                type: String,
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
            }
        }
    }

</script>

<style scoped>
    #db-body {
        margin-left: 300px;
    }
</style>