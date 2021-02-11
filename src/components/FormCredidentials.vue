<template>
    <div id="auth-form" class="mx-2 mt-2">
        Authentification :
        <input type="text" v-model="creds.username" class="w-100 my-1 text-center form-control form-control-sm" placeholder="Utilisateur" @keyup="updateCreds()"/>
        <input type="password" v-model="creds.password" class="w-100 text-center form-control form-control-sm" placeholder="Mot de passe" @keyup="updateCreds()"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            creds: {
                username: 'admin',
                password: 'admin',
            }
        }
    },

    created() {
        this.updateCreds();
    },

    methods: {
        updateCreds: function() {
            this.$emit('update-creds', this.creds);
        }
    }
}
</script>

<style scoped>

</style>