<template>
  <v-app>
    <v-main>
      <v-container
        fluid
        fill-height
        class="d-flex align-center justify-center"
        style="background-color: #0f0f0f"
      >
        <div class="text-center">
          <img
            src="../assets/logolocacao.png"
            alt="logo-carromais"
            height="auto"
            width="550px"
            class="text-center"
            style="margin-top: 10px"
          />
        </div>
        <div class="d-flex flex-column position-relative">
          <v-alert
            dense
            outlined
            type="error"
            class="alert-overlay"
            v-if="this.mensagem != ''"
            >{{ this.mensagem }}
          </v-alert>

          <v-card
            width="450"
            color="#1e1e1e"
            class="pa-6 py-12"
            elevation="2"
            style="border-radius: 12px"
          >
            <h4 style="color: white; margin-bottom: 20px">Faça seu login</h4>
            <form @submit.prevent="login()" method="POST">
              <!-- Email -->
              <div class="mb-4">
                <span class="label">Email</span>
                <v-text-field
                  dark
                  outlined
                  dense
                  class="mt-1"
                  :rules="[rules.required, rules.email]"
                  v-model="email"
                />
              </div>

              <!-- Password -->
              <div class="mb-4">
                <span class="label">Senha</span>
                <v-text-field
                  dark
                  outlined
                  dense
                  class="mt-1"
                  :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show1 ? 'text' : 'password'"
                  @click:append="show1 = !show1"
                  :rules="[rules.required, rules.min]"
                  v-model="password"
                />
              </div>

              <!-- Remember + Forgot -->
              <div class="d-flex align-center justify-space-between mb-4">
                <v-checkbox
                  dark
                  dense
                  hide-details
                  label="Lembre-se de mim"
                  class="ma-0 pa-0"
                />
                <a href="#" class="forgot-link">Esqueceu a senha?</a>
              </div>

              <!-- Button -->
              <v-btn
                block
                color="primary"
                height="45"
                style="border-radius: 8px"
                type="submit"
              >
                Login
              </v-btn>
            </form>
          </v-card>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "LoginComponent",
  data() {
    return {
      show1: false,
      email: "",
      password: "",
      mensagem: "",

      baseUrl: "http://localhost:8000/api/auth/login",
      rules: {
        required: (value) => !!value || "Campo obrigatório.",
        min: (v) => v.length >= 8 || "Min 8 caracteres",
        email: (value) => {
          const pattern =
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
          return pattern.test(value) || "E-mail inválido.";
        },
      },
    };
  },
  methods: {
    login() {
      let body = {
        email: this.email,
        password: this.password,
      };

      axios
        .post(this.baseUrl, body)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.mensagem = error.response.data.Erro;
          console.log(this.mensagem);
          console.log(error);
        });
    },
  },
};
</script>

<style scoped>
.label {
  color: #b3b3b3;
  font-size: 14px;
  text-align: left;
}

.forgot-link {
  color: #42a5f5;
  font-size: 14px;
  text-decoration: none;
}

.forgot-link:hover {
  text-decoration: underline;
}

.position-relative {
  position: relative;
}

.alert-overlay {
  position: absolute;
  top: -60px; /* ajusta a altura */
  width: 100%;
}
</style>
