<template>
  <div class="login-container">
    <h2>Login</h2>
    
    <!-- Login Form -->
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="username">Username</label>
        <input type="text" id="username" v-model="username" required placeholder="Enter your username">
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" v-model="password" required placeholder="Enter your password">
      </div>
      
      <button type="submit" class="login-btn">Login</button>
    </form>

    <!-- Google Sign-In Button -->
    <button @click="onGoogleSignIn" class="google-btn">Sign in with Google</button>

    <!-- Error Message -->
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
  </div>
</template>

<script>
import { createClient } from '@supabase/supabase-js';

// Initialize Supabase client with your credentials
const supabase = createClient(
  'https://gsiimjlxctkwotcedhxt.supabase.co',
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdzaWltamx4Y3Rrd290Y2VkaHh0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3MzI2NzcwMzgsImV4cCI6MjA0ODI1MzAzOH0.YchWDd0bA1JME45t2qXZTKCXbv0dIYfc1qMBhLxvTnk'
);

export default {
  data() {
    return {
      username: '',
      password: '',
      errorMessage: '', // For displaying error messages
    };
  },
  methods: {
    async onGoogleSignIn() {
      try {
        // Trigger Google OAuth login with Supabase
        const { error } = await supabase.auth.signInWithOAuth({
          provider: 'google',
          options: {
            redirectTo: window.location.origin, // Redirect back to your application after login
          },
        });

        if (error) {
          console.error('Error during Google sign-in:', error.message);
          this.errorMessage = `Google sign-in failed: ${error.message}`;
          return;
        }

        console.log('Redirecting to Google for sign-in...');
      } catch (error) {
        console.error('An unexpected error occurred:', error.message);
        this.errorMessage = 'An unexpected error occurred. Please try again later.';
      }
    },

    handleSubmit() {
      // Handle the regular login form submission
      console.log('Login form submitted with:', this.username, this.password);
    },
  },
};
</script>

<style scoped>
.login-container {
  text-align: center;
  padding: 50px;
  max-width: 400px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  font-size: 14px;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  width: 100%;
  margin-top: 10px;
}

.login-btn {
  background-color: #4285f4;
  color: white;
}

.login-btn:hover {
  background-color: #357ae8;
}

.google-btn {
  padding: 10px 20px;
  background-color: #4285f4;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.google-btn:hover {
  background-color: #357ae8;
}

.error-message {
  color: red;
  margin-top: 20px;
}
</style>
