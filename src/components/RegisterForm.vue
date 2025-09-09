<template>
  <div class="register-container">
    <div class="register-card">
      <h2>Register</h2>
      <p>Please login to continue to your account.</p>
      <form @submit.prevent="handleRegister">
        <label>Email</label>
        <input 
          type="email" 
          placeholder="you@example.com" 
          v-model="registerData.email"
          required 
        />

        <label>NIP</label>
        <input 
          type="text" 
          placeholder="NIP" 
          v-model="registerData.nip"
          required 
        />

        <label>Username</label>
        <input 
          type="text" 
          placeholder="username" 
          v-model="registerData.username"
          required 
        />

        <label>Nomor Telepon</label>
        <input 
          type="text" 
          placeholder="nomor telepon" 
          v-model="registerData.phone"
          required 
        />

        <label>Password</label>
        <input 
          type="password" 
          placeholder="create a Password" 
          v-model="registerData.password"
          required 
        />

        <!-- tombol register/login -->
        <button type="submit">Register</button>
        
        <!-- Link untuk switch ke login -->
        <p class="switch-form">
          Already have an account? 
          <a href="#" @click.prevent="$emit('switchToLogin')">Login here</a>
        </p>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "RegisterForm",
  emits: ['registerSuccess', 'switchToLogin'],
  data() {
    return {
      registerData: {
        email: '',
        nip: '',
        username: '',
        phone: '',
        password: ''
      }
    }
  },
  methods: {
    async handleRegister() {
      try {
        // Validasi sederhana
        if (!this.registerData.email || !this.registerData.password) {
          alert('Please fill in all required fields');
          return;
        }

        console.log('Register data:', this.registerData);
        
        // Simulasi API call register
        // const response = await fetch('/api/register', {
        //   method: 'POST',
        //   headers: { 'Content-Type': 'application/json' },
        //   body: JSON.stringify(this.registerData)
        // });
        
        // Simulasi register berhasil setelah 1 detik
        setTimeout(() => {
          alert('Registration successful! Redirecting to login...');
          
          // Reset form
          this.registerData = {
            email: '',
            nip: '',
            username: '',
            phone: '',
            password: ''
          };
          
          // Emit event ke parent untuk pindah ke login
          this.$emit('registerSuccess');
        }, 1000);

      } catch (error) {
        console.error('Registration failed:', error);
        alert('Registration failed. Please try again.');
      }
    }
  }
};
</script>

<style scoped>
/* Background container */
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #fdfdfd;
}

/* Card */
.register-card {
  background: #fff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
  width: 320px;
  text-align: left;
}

/* Title */
.register-card h2 {
  margin: 0 0 5px;
  font-size: 24px;
  font-weight: bold;
}

/* Subtitle */
.register-card p {
  font-size: 14px;
  color: #666;
  margin-bottom: 20px;
}

/* Input label */
.register-card label {
  font-size: 13px;
  font-weight: bold;
  display: block;
  margin-bottom: 4px;
}

/* Input */
.register-card input {
  width: 100%;
  padding: 10px;
  margin-bottom: 14px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  box-sizing: border-box;
}

/* Tombol */
.register-card button {
  display: inline-block;
  width: auto;
  padding: 10px 22px;
  background: #2979ff;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
  transition: 0.3s;
}

.register-card button:hover {
  background: #c0a115;
}

/* Link untuk switch form */
.switch-form {
  text-align: center;
  margin-top: 15px;
  font-size: 14px;
  color: #666;
}

.switch-form a {
  color: #2979ff;
  text-decoration: none;
}

.switch-form a:hover {
  text-decoration: underline;
}
</style>
