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
              v-model="username"
              label="Username"
              :rules="rules"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              label="Password"
              :rules="rules"
              required
            ></v-text-field>
            <v-text-field
              v-model="confirm_password"
              label="Confirm Password"
              required
            ></v-text-field>
            <v-btn type="submit" @click="register">
              Register
            </v-btn>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
<script>
  export default {
    auth: false,
    
    data: () => ({
      email: '',
      password: '',
      confirm_password: '',
      emailRules: [
        v => !!v || 'This field is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      rules: [
        v => !!v || 'This field is required',
      ]
    }),
    methods: {
      async register() {
        try {
          await this.$axios.post('register', {
            email: this.email,
            username: this.username,
            password: this.password
          })

          this.$router.push('/login')
        } catch (e) {
          this.error = e.response.data.message
        }
      }
    }
  }
</script>