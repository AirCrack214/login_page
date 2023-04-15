<template>
  <div class="registration-form">
    <h2>Registration Form</h2>
    <form @submit.prevent="submitForm" ref="form">
      <div class="form-group">
        <label for="login">Login:</label>
        <input type="text" id="login" v-model="login" :class="{ 'is-invalid': isLoginInvalid }" required>
        <div v-if="login && isLoginInvalid" class="error">Please enter a valid login.</div>
      </div>
      
      <div class="form-group">
        <label for="phone">Phone:</label>
        <input type="tel" id="phone" v-model="phone" :class="{ 'is-invalid': isPhoneInvalid }" pattern="[0-9]{11}">
        <div v-if="phone && !validPhone" class="error">Please enter a valid phone number.</div>
      </div>
      
      <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="password" :class="{ 'is-invalid': isPasswordInvalid }" required>
        <div v-if="password && isPasswordInvalid" class="error">Please enter a valid password.</div>
      </div>
      
      <button type="submit">Register</button>
    </form>
    <div v-if="showSuccessMessage" class="success">Form submitted successfully!</div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      login: '',
      phone: '',
      password: '',
      showSuccessMessage: false
    }
  },
  computed: {
    validPhone() {
      if (!this.phone) return true;

      const isValidLength = this.phone.length === 11;
      const isValidCharacters = /^[0-9]+$/.test(this.phone);
      return isValidLength && isValidCharacters;
    },
    isLoginInvalid() {
      if (!this.login) return false;

      const isValidLength = this.login.length > 5;
      const isValidCharacters = /^[a-zA-Z0-9_]+$/.test(this.login);
      return !isValidLength || !isValidCharacters;
    },
    isPasswordInvalid() {
      if (!this.password) return false;
      
      return this.password.length <= 6;
    },
    isPhoneInvalid() {
      return this.phone && !this.validPhone;
    }
  },
  methods: {
    submitForm() {
      if (this.$refs.form.checkValidity() && !this.isLoginInvalid && !this.isPasswordInvalid) {
        const formData = {
          login: this.login,
          phone: this.phone,
          password: this.password,
        }
        console.log(formData)
        // send form data to the server

        this.showSuccessMessage = true
        
        this.login = '' 
        this.phone = ''
        this.password = ''

        setTimeout(() => {
          this.showSuccessMessage = false
        }, 3000)
      } else {
        alert("Please fill in all required fields.");
      }
    },
  },
}
</script>


<style scoped>
  .error {
    color: red;
    margin-left: 1rem;
    display: inline-block;
    position: absolute;
    white-space: nowrap;
  }
  
  .is-invalid {
    border-color: red;
  }

  .success {
    color: green;
    margin-top: 0.5rem;
    position: absolute;
    white-space: nowrap;
  }
</style>