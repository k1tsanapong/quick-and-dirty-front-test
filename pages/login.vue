<template>
  <v-container>
    <v-row class="my-12" justify="center" align="center">
      <div class="text-h1">Admin</div>
    </v-row>

    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-text-field
          v-model="user.username"
          outlined
          label="User"
          prepend-inner-icon="mdi-account"
        ></v-text-field>
      </v-col>

      <v-col cols="12">
        <v-text-field
          v-model="user.password"
          outlined
          label="Password"
          prepend-inner-icon="mdi-lock"
          type="password"
        ></v-text-field>
      </v-col>

      <v-col cols="12">
        <v-btn @click="signIn"> Login </v-btn>
      </v-col>
    </v-row>

    <v-row justify="center" align="center">
      <!-- <v-btn to="/activities"> Login </v-btn> -->
    </v-row>

    <v-snackbar class="mb-5" v-model="snackbar" :timeout="2000">
      {{ snackbarText }}

      <template v-slot:action="{ attrs }">
        <v-btn color="blue" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
const localStorage = window.localStorage;

export default {
  layout: "blank",
  name: "Login",
  data: () => ({
    user: {
      username: "",
      password: "",
    },
    snackbar: false,
    snackbarText: "",
  }),
  created() {},
  methods: {
    async signIn(e) {
      e.preventDefault();

      const payload = {
        username: this.user.username,
        password: this.user.password,
      };

      console.log(this.user.username, this.user.password);

      try {
        await this.$auth.loginWith("local", {
          data: payload,
        });
        this.$router.push("/");
      } catch (error) {
        if (error.response === undefined) {
          this.snackbarText = error.message;
        } else {
          this.snackbarText = error.response.data.message;
        }

        this.snackbar = true;
      }
    },
  },
};
</script>
