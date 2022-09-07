<template>
  <v-container>
    <v-row>
      <v-col cols="6">
        <v-form ref="form">
          <v-text-field
            v-model="email"
            label="Email"
            :rules="emailRules"
            required
          ></v-text-field>
          <v-text-field
            type="password"
            v-model="password"
            label="Password"
            :rules="rules"
            required
          ></v-text-field>
          <v-btn type="submit" @click="login">
            Login
          </v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      email: '',
      password: '',
      emailRules: [
        v => !!v || 'This field is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      rules: [
        v => !!v || 'This field is required',
      ]
    }),
    methods: {
      async login() {
        try {
          await this.$auth.loginWith('laravelSanctum', {
            data: {
              email: this.email,
              password: this.password
            }
          })

          this.$router.push('/')
        } catch (e) {
          this.error = e.response.data.message
        }
      }
    }
  }
</script>