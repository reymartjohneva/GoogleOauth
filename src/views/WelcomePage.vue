<template>
  <div class="dashboard-container">
    <img
      v-if="user?.user_metadata?.avatar_url"
      :src="user.user_metadata.avatar_url"
      alt="Profile Picture"
      class="profile-pic"
    />
    <h2>Welcome, {{ user?.user_metadata?.full_name || user?.email || "User" }}</h2>
    <p v-if="user?.email">Email: {{ user.email }}</p>
    <button @click="onLogout" class="logout-btn">Logout</button>
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
      user: null, // Stores the user's information
    };
  },
  async created() {
    try {
      // Check if the user is logged in
      const {
        data: { session },
      } = await supabase.auth.getSession();

      if (session) {
        this.user = session.user; // Save the user data
      } else {
        this.$router.push("/"); // Redirect to login if not authenticated
      }
    } catch (error) {
      console.error("Error fetching session:", error.message);
    }
  },
  methods: {
    async onLogout() {
      try {
        const { error } = await supabase.auth.signOut();
        if (error) {
          console.error("Logout error:", error.message);
        } else {
          this.$router.push("/"); // Redirect to login page
        }
      } catch (error) {
        console.error("Error during logout:", error.message);
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

.profile-pic {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-bottom: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

h2 {
  font-size: 24px;
  margin-bottom: 10px;
  color: #333;
}

p {
  font-size: 16px;
  margin-bottom: 20px;
  color: #555;
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
