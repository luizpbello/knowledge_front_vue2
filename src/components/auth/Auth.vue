<template>
<div class="auth-content">
    <div class="auth-modal">
        <h1>Luiz Bello<p></p> Knowledge</h1>
        <hr>
        <div class="auth-title"> {{ showSignup ? 'Cadastro' : 'Login' }}</div>

        <input v-if="showSignup" v-model="user.name" placeholder="Nome" type="text">
        <input  v-model="user.email" placeholder="E-mail" type="email">
        <input  v-model="user.password" placeholder="Senha" type="password">
        <input v-if="showSignup" v-model="user.confirmPassword" placeholder="Confirmar Senha" type="password">

        <button v-if="showSignup" @click="signup" class="btn btn-primary">Cadastrar</button>
        <button v-else @click="signin" class="btn btn-primary">Entrar</button>

        <a href @click.prevent="showSignup = !showSignup">
        <span v-if="showSignup">Já possui uma conta?</span>
        <span v-else>Não possui uma conta? Registre-se aqui</span>
        </a>
    </div>
</div>
  
</template>

<script>
import { baseApiUrl, showError, userKey } from '../../global'
import axios from 'axios'


export default {
    name:'Auth',
    data() {
        return {
            showSignup : false,
            user: {}
        }
    },
    methods:{
        signin() {
            axios.post(`${baseApiUrl}/signin`,this.user)
            .then(res => {
                this.$store.commit('setUser', res.data)
                localStorage.setItem(userKey, JSON.stringify(res.data))
                this.$router.push({path: '/'})
            })
            .catch(showError)
        },
        signup() {
            axios.post(`${baseApiUrl}/signup`,this.user)
            .then(() => {
                this.$toasted.global.defaultSuccess()
                this.user = {}
                this.showSignup = false
            })
            .catch(showError)
        }

    }

}
</script>

<style>

.auth-content {
    display:flex;
    height:100%;
    justify-content:center;
    align-items:center;
}

.auth-modal {
    background-color:#FFF;
    width:350px;
    padding:35px;
    border-radius:5px;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.15);

    display:flex;
    flex-direction:column;
    align-items:center;
}

.auth-title {
    font-size: 1.2rem;
    font-weight: 100;
    margin-bottom: 15px;
}

.auth-modal h1{
    font-size: 1.5rem;
    font-weight: 700;
    margin-top:10px;
}

.auth-modal input {
    border: 1px soliod #bbb;
    width:100%;
    padding:3px 8px;
    margin-bottom:15px;
    outline:none;

}

.auth-modal button{
    width:100%;
    padding:10px;
    margin-bottom:10px;
    background-color: orangered;
    border:none;    
}

.auth-modal button:hover{
    background-color: #ff7300;
    color: #FFF;
}

.auth.modal a{
    margin-top: 35px;
    text-decoration: none;
    color: #bbb;

}

.auth-modal span{
    font-size: 0.8rem;
    font-weight: 100;
    margin-top:25px;
    color:black;
    text-decoration:none;
}

.auth-modal hr {
    border:0;
    width:100%;
    height:1px;
    background-image: linear-gradient( to right,
     rgba(120, 120, 120, 0.),
rgba(120, 120,120, 0.75),
    rgba(120, 120, 120 0) );
}

</style>