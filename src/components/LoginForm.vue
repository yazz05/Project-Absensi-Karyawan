<template>
  <div class="login-container">
    <div class="login-form">
      <h2 class="login-title">Login</h2>
      <p class="login-subtitle">Please login to continue to your account.</p>
      
      <form @submit.prevent="handleLogin">
        <!-- Email Field -->
        <div class="form-group">
          <label for="email">Email</label>
          <input
            type="email"
            id="email"
            v-model="loginData.email"
            placeholder="your@example.com"
            required
          />
        </div>

        <!-- NIP Field -->
        <div class="form-group">
          <label for="nip">NIP</label>
          <div class="input-wrapper">
            <input
              :type="showNip ? 'text' : 'password'"
              id="nip"
              v-model="loginData.nip"
              placeholder="NIP"
              required
            />
            <button 
              type="button" 
              class="toggle-visibility"
              @click="showNip = !showNip"
            >
              👁️
            </button>
          </div>
        </div>

        <!-- Password Field -->
        <div class="form-group">
          <label for="password">Password</label>
          <div class="input-wrapper">
            <input
              :type="showPassword ? 'text' : 'password'"
              id="password"
              v-model="loginData.password"
              placeholder="Password"
              required
            />
            <button 
              type="button" 
              class="toggle-visibility"
              @click="showPassword = !showPassword"
            >
              👁️
            </button>
          </div>
        </div>

        <!-- Keep me logged in -->
        <div class="form-group checkbox-group">
          <input 
            type="checkbox" 
            id="keepLoggedIn" 
            v-model="loginData.keepLoggedIn"
          />
          <label for="keepLoggedIn">Keep me logged in</label>
        </div>

        <!-- Submit Button -->
        <button type="submit" class="login-button">
          Login
        </button>
        
        <!-- Link untuk switch ke register -->
        <p class="switch-form">
          Don't have an account? 
          <a href="#" @click.prevent="$emit('switchToRegister')">Register here</a>
        </p>
      </form>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
  name: 'LoginForm',
  emits: ['switchToRegister', 'loginSuccess'],
  setup(props, { emit }) {
    const showPassword = ref(false)
    const showNip = ref(false)
    
    const loginData = reactive({
      email: '',
      nip: '',
      password: '',
      keepLoggedIn: false
    })

    const handleLogin = async () => {
      try {
        // Validasi sederhana
        if (!loginData.email || !loginData.nip || !loginData.password) {
          alert('Please fill in all required fields');
          return;
        }

        console.log('Login data:', loginData)
        
        // Simulasi API call login
        // const response = await fetch('/api/login', {
        //   method: 'POST',
        //   headers: { 'Content-Type': 'application/json' },
        //   body: JSON.stringify({
        //     email: loginData.email,
        //     nip: loginData.nip,
        //     password: loginData.password,
        //     keepLoggedIn: loginData.keepLoggedIn
        //   })
        // });
        
        // Simulasi login berhasil setelah 1 detik
        setTimeout(() => {
          alert('Login successful! Redirecting to OTP verification...');
          
          // Simpan data user sebelum reset
          const userData = {
            email: loginData.email,
            nip: loginData.nip
          };
          
          // Reset form setelah emit
          Object.assign(loginData, {
            email: '',
            nip: '',
            password: '',
            keepLoggedIn: false
          });
          
          // Emit event ke parent dengan data yang sudah disimpan
          emit('loginSuccess', userData);
          
        }, 1000);

      } catch (error) {
        console.error('Login failed:', error);
        alert('Login failed. Please check your credentials and try again.');
      }
    }

    return {
      loginData,
      showPassword,
      showNip,
      handleLogin
    }
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 20px;
}

.login-form {
  background: white;
  padding: 2.5rem;
  border-radius: 8px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  border: 1px solid #e0e0e0;
}

.login-title {
  font-size: 1.75rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #333;
  text-align: left;
}

.login-subtitle {
  color: #666;
  margin-bottom: 2rem;
  font-size: 0.9rem;
  line-height: 1.4;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #333;
  font-size: 0.9rem;
}

.form-group input[type="email"],
.form-group input[type="text"],
.form-group input[type="password"] {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.9rem;
  transition: border-color 0.2s;
  box-sizing: border-box;
}

.form-group input:focus {
  outline: none;
  border-color: #4285f4;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.input-wrapper input {
  padding-right: 3rem;
}

.toggle-visibility {
  position: absolute;
  right: 0.75rem;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  color: #666;
  padding: 0;
  height: 20px;
  width: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.toggle-visibility:hover {
  color: #333;
}

.checkbox-group {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.checkbox-group input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.checkbox-group label {
  margin: 0;
  font-weight: normal;
  cursor: pointer;
  font-size: 0.9rem;
}

.login-button {
  width: 100%;
  background-color: #4285f4;
  color: white;
  padding: 0.875rem;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.login-button:hover {
  background-color: #3367d6;
}

.login-button:active {
  background-color: #2851a3;
}

/* Link untuk switch form */
.switch-form {
  text-align: center;
  margin-top: 15px;
  font-size: 0.9rem;
  color: #666;
}

.switch-form a {
  color: #4285f4;
  text-decoration: none;
}

.switch-form a:hover {
  text-decoration: underline;
}
</style>
