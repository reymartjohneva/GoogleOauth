<template>
    <div class="dashboard-container">
      <h2>Welcome, {{ user?.email || "User" }}</h2>
      <button @click="onLogout" class="logout-btn">Logout</button>
    </div>
  </template>
  
  <script>
  import { createClient } from "@supabase/supabase-js";
  
  // Initialize Supabase client with your credentials
  const supabase = createClient(
    "https://gsiimjlxctkwotcedhxt.supabase.co",
    "YOUR_SUPABASE_ANON_KEY"
  );
  
  export default {
    data() {
      return {
        user: null,
      };
    },
    async created() {
      // Check if the user is logged in
      const {
        data: { session },
      } = await supabase.auth.getSession();
      if (session) {
        this.user = session.user;
      } else {
        this.$router.push("/"); // Redirect to login if not authenticated
      }
    },
    methods: {
      async onLogout() {
        const { error } = await supabase.auth.signOut();
        if (error) {
          console.error("Logout error:", error.message);
        } else {
          this.$router.push("/"); // Redirect to login page
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .dashboard-container {
    text-align: center;
    padding: 50px;
    max-width: 400px;
    margin: 0 auto;
    border: 1px solid #ccc;
    border-radius: 10px;
    background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
  }
  
  .logout-btn {
    background-color: #e74c3c;
    color: white;
    padding: 12px 20px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    font-weight: bold;
    transition: all 0.3s ease;
  }
  
  .logout-btn:hover {
    background-color: #c0392b;
    box-shadow: 0 4px 6px rgba(231, 76, 60, 0.3);
  }
  </style>
  