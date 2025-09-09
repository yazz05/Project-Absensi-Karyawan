<template>
  <div class="app">
    <!-- Login Form -->
    <LoginForm 
      v-if="currentView === 'login'" 
      @switchToRegister="currentView = 'register'"
      @loginSuccess="handleLoginSuccess"
    />
    
    <!-- Register Form -->
    <RegisterForm 
      v-if="currentView === 'register'" 
      @registerSuccess="currentView = 'login'"
      @switchToLogin="currentView = 'login'" 
    />
    
    <!-- OTP Verification -->
    <OTPVerification 
      v-if="currentView === 'otp'"
      :userEmail="currentUser.email"
      @otpVerified="handleOTPVerified"
      @backToLogin="currentView = 'login'"
    />
    
    <!-- Dashboard -->
    <Dashboard 
      v-if="currentView === 'dashboard'"
      :user="currentUser"
      @logout="handleLogout"
    />
  </div>
</template>

<script>
import RegisterForm from "./components/RegisterForm.vue";
import LoginForm from "./components/LoginForm.vue";
import OTPVerification from "./components/OTPVerification.vue";
import Dashboard from "./components/Dashboard.vue";

export default {
  components: { 
    RegisterForm,
    LoginForm,
    OTPVerification,
    Dashboard
  },
  data() {
    return {
      currentView: 'register', // Start with register
      currentUser: {
        email: '',
        nip: '',
        username: ''
      }
    }
  },
  methods: {
    handleLoginSuccess(userData) {
      console.log('Login success:', userData);
      this.currentUser = userData;
      this.currentView = 'otp'; // Redirect to OTP after login
    },
    
    handleOTPVerified(data) {
      console.log('OTP verified:', data);
      // Redirect to dashboard after OTP verification
      this.currentView = 'dashboard';
    },
    
    handleLogout() {
      // Clear user data and redirect to login
      this.currentUser = {
        email: '',
        nip: '',
        username: ''
      };
      localStorage.removeItem('user');
      sessionStorage.removeItem('user');
      this.currentView = 'login';
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
}

.app {
  min-height: 100vh;
  background-color: #f5f5f5;
}
</style>
