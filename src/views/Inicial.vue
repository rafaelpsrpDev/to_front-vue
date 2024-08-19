<template>
    <div>
        <h1>Tela Inicial</h1>

        <form @submit.prevent>

            <input type="text" v-model="titulo" name="titulo" placeholder="Digite o titulo da tarefa">
            <input type="text" v-model="descricao" name="descricao" placeholder="Digite a descricao da tarefa">

            <button @click="criarTarefa">Acessar</button>
        </form>
    </div>
</template>

<script>
    import Cookie from 'js-cookie';
    import axios from 'axios';

    export default {
        data: () => ({
            titulo: '',
            descricao: '',
            responseToken: null
        }),

        methods: {
            async criarTarefa() {
                const headers = {'Authorization': `Bearer ${this.responseToken}` }
                const response = await axios.post('http://localhost:8000/api/createTarefa', {
                    titulo: this.titulo, 
                    descricao: this.descricao
                }, {headers}  )
                console.log(response)
            }
        },  

        mounted() {
            this.responseToken = Cookie.get('token');
            if (typeof this.responseToken === 'undefined') {
                this.$router.push({
                    path: '/'
                })
            }
        }
    }
</script>

<style>

</style>