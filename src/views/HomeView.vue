<template>
  <div class="login-container">
    <h2>Login</h2>
    
    <!-- Login Form -->
    <form @submit.prevent="handleLogin">
      <div class="form-group">
        <label for="username">Username</label>
        <input 
          type="text" 
          id="username" 
          v-model="username" 
          required 
          placeholder="Enter your username" 
        />
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input 
          type="password" 
          id="password" 
          v-model="password" 
          required 
          placeholder="Enter your password" 
        />
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
import { createClient } from "@supabase/supabase-js";

// Initialize Supabase client with your credentials
const supabase = createClient(
  "https://gsiimjlxctkwotcedhxt.supabase.co",
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdzaWltamx4Y3Rrd290Y2VkaHh0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3MzI2NzcwMzgsImV4cCI6MjA0ODI1MzAzOH0.YchWDd0bA1JME45t2qXZTKCXbv0dIYfc1qMBhLxvTnk"
);

export default {
  data() {
    return {
      username: "",
      password: "",
      errorMessage: "", // For displaying error messages
    };
  },
  methods: {
    // Handle Google sign-in
    async onGoogleSignIn() {
      try {
        const { error } = await supabase.auth.signInWithOAuth({
          provider: "google",
          options: {
            redirectTo: `${window.location.origin}/welcome`, // Redirect to welcome page after login
          },
        });

        if (error) {
          console.error("Error during Google sign-in:", error.message);
          this.errorMessage = `Google sign-in failed: ${error.message}`;
          return;
        }

        console.log("Redirecting to Google for sign-in...");
      } catch (error) {
        console.error("An unexpected error occurred:", error.message);
        this.errorMessage = "An unexpected error occurred. Please try again later.";
      }
    },

    // Handle form login
    async handleLogin() {
      try {
        const { error, data } = await supabase.auth.signInWithPassword({
          email: this.username,
          password: this.password,
        });

        if (error) {
          this.errorMessage = "Login failed. Check your credentials and try again.";
          console.error("Login error:", error.message);
          return;
        }

        console.log("Login successful:", data);
        window.location.href = "/welcome"; // Redirect to welcome page
      } catch (error) {
        this.errorMessage = "An unexpected error occurred. Please try again later.";
        console.error("Unexpected error:", error.message);
      }
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
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
}

h2 {
  margin-bottom: 20px;
  color: #333;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  font-size: 14px;
  margin-bottom: 5px;
  color: #555;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
  box-sizing: border-box;
}

input:focus {
  border-color: #4285f4;
  box-shadow: 0 0 5px rgba(66, 133, 244, 0.5);
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  width: 100%;
  margin-top: 10px;
  color: white;
  transition: background-color 0.3s ease;
}

.login-btn {
  background-color: #4285f4;
}

.login-btn:hover {
  background-color: #357ae8;
}

.google-btn {
  padding: 10px 20px;
  background-color: #db4437;
  color: white;
  margin-top: 15px;
}

.google-btn:hover {
  background-color: #c33627;
}

.error-message {
  color: red;
  margin-top: 20px;
}
</style>
