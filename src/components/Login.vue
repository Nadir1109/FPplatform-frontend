<template>
  <div class="login-container">
    <div class="login-form">
      <h2>Inloggen op het platform</h2>
      <form @submit.prevent="submitLogin">
        <div class="form-group">
          <label for="email">Email</label>
          <input
              type="text"
              id="email"
              v-model="email"
              placeholder="Email"
              required
          />
        </div>

        <div class="form-group">
          <label for="password">Wachtwoord</label>
          <input
              type="password"
              id="password"
              v-model="password"
              placeholder="Wachtwoord"
              required
          />
        </div>

        <button type="submit" class="login-btn">Inloggen</button>

        <p v-if="message" class="login-message">{{ message }}</p>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
      message: ''
    };
  },
  methods: {
    async submitLogin() {
      try {
        // Maak een POST-aanvraag naar je login API
        const response = await axios.post('http://localhost:8080/api/auth/login', {
          email: this.email,
          password: this.password
        });

        // Sla de token op in localStorage
        localStorage.setItem('authToken', response.data.token);

        // Leeg de velden na succesvolle login
        this.clearFields();

        // Navigeer naar de homepagina na succesvolle login
        this.$router.push('/home');
      } catch (error) {
        console.error(error);
        this.message = 'Ongeldige inloggegevens, probeer opnieuw.';
      }
    },
    clearFields() {
      this.email = '';
      this.password = '';
    }
  }
};
</script>
