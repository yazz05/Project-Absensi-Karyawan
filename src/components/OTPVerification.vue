<template>
  <div class="otp-container">
    <div class="otp-card">
      <h2>OTP Verification Code</h2>
      <p class="otp-subtitle">
        We have sent the code to {{ userEmail }}
      </p>
      
      <!-- OTP Input Fields -->
      <div class="otp-inputs">
        <input
          v-for="(digit, index) in otpDigits"
          :key="index"
          :ref="'otpInput' + index"
          v-model="otpDigits[index]"
          type="text"
          maxlength="1"
          class="otp-input"
          @input="handleInput(index, $event)"
          @keydown="handleKeydown(index, $event)"
          @paste="handlePaste($event)"
        />
      </div>
      
      <!-- Resend Code Link -->
      <p class="resend-section">
        Didn't recieve a code? 
        <a href="#" @click.prevent="resendCode" class="resend-link">Resend code</a>
      </p>
      
      <!-- Confirm Button -->
      <button 
        type="button" 
        class="confirm-button"
        @click="confirmOTP"
        :disabled="!isOTPComplete"
      >
        Confirm
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OTPVerification',
  props: {
    userEmail: {
      type: String,
      default: 'user@example.com'
    }
  },
  emits: ['otpVerified', 'backToLogin'],
  data() {
    return {
      otpDigits: ['', '', '', '', '', ''],
      countdown: 60,
      countdownInterval: null
    }
  },
  computed: {
    isOTPComplete() {
      return this.otpDigits.every(digit => digit !== '');
    },
    fullOTP() {
      return this.otpDigits.join('');
    }
  },
  mounted() {
    // Auto focus first input
    this.$nextTick(() => {
      if (this.$refs.otpInput0 && this.$refs.otpInput0[0]) {
        this.$refs.otpInput0[0].focus();
      }
    });
    
    // Start countdown
    this.startCountdown();
  },
  beforeUnmount() {
    if (this.countdownInterval) {
      clearInterval(this.countdownInterval);
    }
  },
  methods: {
    handleInput(index, event) {
      const value = event.target.value;
      
      // Only allow numbers
      if (!/^\d*$/.test(value)) {
        this.otpDigits[index] = '';
        return;
      }
      
      this.otpDigits[index] = value;
      
      // Auto focus next input
      if (value && index < 5) {
        this.$nextTick(() => {
          const nextInput = this.$refs['otpInput' + (index + 1)];
          if (nextInput && nextInput[0]) {
            nextInput[0].focus();
          }
        });
      }
    },
    
    handleKeydown(index, event) {
      // Handle backspace
      if (event.key === 'Backspace' && !this.otpDigits[index] && index > 0) {
        this.$nextTick(() => {
          const prevInput = this.$refs['otpInput' + (index - 1)];
          if (prevInput && prevInput[0]) {
            prevInput[0].focus();
          }
        });
      }
      
      // Handle arrow keys
      if (event.key === 'ArrowLeft' && index > 0) {
        this.$refs['otpInput' + (index - 1)][0].focus();
      }
      if (event.key === 'ArrowRight' && index < 5) {
        this.$refs['otpInput' + (index + 1)][0].focus();
      }
    },
    
    handlePaste(event) {
      event.preventDefault();
      const pasteData = event.clipboardData.getData('text');
      const digits = pasteData.replace(/\D/g, '').split('').slice(0, 6);
      
      digits.forEach((digit, index) => {
        if (index < 6) {
          this.otpDigits[index] = digit;
        }
      });
      
      // Focus last filled input or first empty
      const lastIndex = Math.min(digits.length, 5);
      this.$nextTick(() => {
        const targetInput = this.$refs['otpInput' + lastIndex];
        if (targetInput && targetInput[0]) {
          targetInput[0].focus();
        }
      });
    },
    
    confirmOTP() {
      if (!this.isOTPComplete) {
        alert('Please enter complete OTP code');
        return;
      }
      
      console.log('OTP entered:', this.fullOTP);
      
      // Simulate OTP verification
      if (this.fullOTP === '201200' || this.fullOTP === '123456') {
        alert('OTP verified successfully!');
        this.$emit('otpVerified', { 
          email: this.userEmail, 
          otp: this.fullOTP 
        });
      } else {
        alert('Invalid OTP code. Please try again.');
        this.clearOTP();
      }
    },
    
    resendCode() {
      if (this.countdown > 0) {
        alert(`Please wait ${this.countdown} seconds before resending`);
        return;
      }
      
      console.log('Resending OTP to:', this.userEmail);
      alert('New OTP code has been sent to your email');
      
      // Reset countdown
      this.countdown = 60;
      this.startCountdown();
      
      // Clear current OTP
      this.clearOTP();
    },
    
    clearOTP() {
      this.otpDigits = ['', '', '', '', '', ''];
      this.$nextTick(() => {
        if (this.$refs.otpInput0 && this.$refs.otpInput0[0]) {
          this.$refs.otpInput0[0].focus();
        }
      });
    },
    
    startCountdown() {
      this.countdownInterval = setInterval(() => {
        if (this.countdown > 0) {
          this.countdown--;
        } else {
          clearInterval(this.countdownInterval);
        }
      }, 1000);
    }
  }
}
</script>

<style scoped>
.otp-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 20px;
}

.otp-card {
  background: white;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  text-align: center;
}

.otp-card h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #333;
}

.otp-subtitle {
  color: #666;
  margin-bottom: 2rem;
  font-size: 0.9rem;
  line-height: 1.4;
}

.otp-inputs {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 2rem;
}

.otp-input {
  width: 45px;
  height: 45px;
  text-align: center;
  font-size: 1.25rem;
  font-weight: 600;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  background-color: #f8f9fa;
  transition: all 0.2s;
}

.otp-input:focus {
  outline: none;
  border-color: #4285f4;
  background-color: white;
  box-shadow: 0 0 0 3px rgba(66, 133, 244, 0.1);
}

.otp-input:not(:placeholder-shown) {
  border-color: #4285f4;
  background-color: white;
}

.resend-section {
  color: #666;
  margin-bottom: 2rem;
  font-size: 0.9rem;
}

.resend-link {
  color: #4285f4;
  text-decoration: none;
  font-weight: 500;
}

.resend-link:hover {
  text-decoration: underline;
}

.confirm-button {
  width: 100%;
  background-color: #4285f4;
  color: white;
  padding: 0.875rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.confirm-button:hover:not(:disabled) {
  background-color: #3367d6;
}

.confirm-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

@media (max-width: 480px) {
  .otp-inputs {
    gap: 0.5rem;
  }
  
  .otp-input {
    width: 40px;
    height: 40px;
    font-size: 1.1rem;
  }
}
</style>
