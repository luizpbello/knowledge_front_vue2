<template>
  <div class="auth-content">
    <div class="auth-modal">
      <h1>
        Luiz Bello
        <p></p>
        Knowledge
      </h1>
      <hr />
      <div class="auth-title">{{ showSignup ? "Cadastro" : "Login" }}</div>

      <b-form-input
        v-if="showSignup"
        id="userName"
        v-model="user.name"
        placeholder="Nome"
        type="text"
        trim
        :state="nameState"
        lazy-validation
      />
      <b-form-invalid-feedback >
        Digite pelo menos 3 letras.
      </b-form-invalid-feedback>
      <b-form-input
        id="userEmail"
        v-model="user.email"
        placeholder="E-mail"
        lazy-validation
        :state="emailState"
        type="email"
      />
      <b-form-invalid-feedback  id="userEmail-live-feedback">
        Digite um email válido.
      </b-form-invalid-feedback>
      <b-form-input
        :state="passwordState"
        v-model="user.password"
        placeholder="Senha"
        type="password"
      />
      <b-form-invalid-feedback id="input-live-feedback">
        Senha minímo 3 dígitos.
      </b-form-invalid-feedback>
      <b-form-input
        v-if="showSignup"
        v-model="user.confirmPassword"
        placeholder="Confirmar Senha"
        type="password"
      />

      <button v-if="showSignup" @click="signup" class="btn btn-primary">
        Cadastrar
      </button>
      <button v-else @click="signin" class="btn btn-primary">Entrar</button>

      <a href @click.prevent="showSignup = !showSignup">
        <span v-if="showSignup">Já possui uma conta?</span>
        <span v-else>Não possui uma conta? Registre-se aqui</span>
      </a>
    </div>
  </div>
</template>

<script>
import { baseApiUrl, showError, userKey } from "../../global";
import axios from "axios";

export default {
  name: "AuthApp",
  computed: {
    nameState() {
      if (!this.user.name.length) {
        return null;
      }
      return this.user.name.length >= 3;
    },
    emailState() {
      if (!this.user.email.length) {
        return null;
      }

      let re =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(this.user.email);
    },
    passwordState() {
      if (!this.user.password.length) {
        return null;
      }

      return this.user.password.length >= 3;
    },
  },
  data() {
    return {
      showSignup: false,
      user: {
        name: "",
        email: "",
        password: "",
        confirmPassword: "",
      },
    };
  },
  methods: {
    signin() {
      axios
        .post(`${baseApiUrl}/signin`, this.user)
        .then((res) => {
          this.$store.commit("setUser", res.data);
          localStorage.setItem(userKey, JSON.stringify(res.data));
          this.$router.push({ path: "/" });
        })
        .catch(showError);
    },
    signup() {
      axios
        .post(`${baseApiUrl}/signup`, this.user)
        .then(() => {
          this.$toasted.global.defaultSuccess();
          this.user = {};
          this.showSignup = false;
        })
        .catch(showError);
    },
  },
};
</script>

<style>
.auth-content {
  display: flex;
  height: 100%;
  justify-content: center;
  align-items: center;
}

.auth-modal {
  background-color: #fff;
  width: 350px;
  padding: 35px;
  border-radius: 5px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.15);

  display: flex;
  flex-direction: column;
  align-items: center;
}

.auth-title {
  font-size: 1.2rem;
  font-weight: 100;
  margin-bottom: 15px;
}

.auth-modal h1 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-top: 10px;
}

.auth-modal input {
  border: 1px solid black;
  width: 100%;
  padding: 3px 8px;
  margin-bottom: 15px;
}

.auth-modal button {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  background-color: orangered;
  border: none;
}

.auth-modal button:hover,
.auth-modal button:focus {
  background-color: #ff7300;
  color: #fff;
}

.auth.modal a {
  margin-top: 35px;
  text-decoration: none;
  color: #bbb;
}

.auth-modal span {
  font-size: 0.8rem;
  font-weight: 100;
  margin-top: 25px;
  color: black;
  text-decoration: none;
}

.auth-modal hr {
  border: 0;
  width: 100%;
  height: 1px;
  background-image: linear-gradient(
    to right,
    rgba(120, 120, 120, 0),
    rgba(120, 120, 120, 0.75),
    rgba(120, 120, 120 0)
  );
}
</style>
