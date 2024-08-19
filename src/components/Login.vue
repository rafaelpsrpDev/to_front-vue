<template>
    <div class="container-login">
            <h1 class="container-label">Login</h1>   
            <form @submit.prevent>

                <input type="email" v-model="email" name="email" placeholder="Digite seu email"/>
                <input type="password" v-model="password" name="pass" placeholder="Digite sua senha">

                <!-- <a href="#">Esqueceu a senha?</a> -->

                <button @click="enviaForm()">Acessar</button>

            </form>
    </div>
</template>

<script>
    import axios from 'axios'
    import Cookie from 'js-cookie'

    export default { 
        data:() => ({
            token: '',
            email: '',
            password: ''
        }),  
        methods: {
            async getStatus() {
                const { data } = await axios.get("http://localhost:8000/api/status", {}
            ); 
                console.log(data);
            },
            
            async enviaForm() {
                try {
                    const data = await axios.post("http://localhost:8000/api/login", {
                        email: this.email,
                        password: this.password
                        });
                    
                    Cookie.remove('token'); 
                
                    if (data.data.authorisation.token) {
                    
                        Cookie.set('token', data.data.authorisation.token); 
                        setTimeout(() => {
                            this.$router.push({
                                path: '/inicial'
                            })
                        }, 2000)

                    }
                } catch (e) {
                    alert('Login Errado');
                }
                
            }
        },
        mounted() {}
    
    }
</script>

<style>
    .container-login {
        height: 200px;
        width: 400px;
        margin: 150px 500px;
        padding: 30px;
        border: 1px ridge rgb(69, 69, 103);
        border-radius: 20px;
    }

    .container-login input {
        height: 30px;
        width: 340px;
        padding: 3.5px;
        font-size: 15px;
        display: block;
        margin: 15px 10px 10px 20px;
        border-radius: 2px;
        border-style: none;
    }

    .container-login button {
        display: block;
        width: 100px;
        height: 40px;
        cursor: pointer;
        margin-left: 150px;
        margin-top: 20px;
        background-color: rgb(99, 99, 247);
        color: white;   
        border-radius: 10px;
        border-style: none;
    }
    
    .container-label {
        text-align: center;
        color: rgb(88, 88, 107);
        font-family: Arial, Helvetica, sans-serif;
    }
</style>