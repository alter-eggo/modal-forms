<template lang="html">
  <modal title="Registration form"
    @close="closeModal">
      <div slot="body">
        <form @submit.prevent="onSubmit">
          <!-- login -->
          <div class="form-item" :class="{ errorInput: $v.login.$error}">
            <label>Login:</label>
            <p class="errorText" v-if="!$v.login.required">Field is required</p>
            <p class="errorText" v-if="!$v.login.minLength">Login must have at least {{ $v.login.$params.minLength.min }} figures!</p>

            <input v-model="login"
            :class="{ error: $v.login.$error }"
            @change="$v.login.$touch()">
          </div>
          <!-- name -->
          <div class="form-item" :class="{ errorInput: $v.name.$error}">
            <label>Name:</label>
            <p class="errorText" v-if="!$v.name.required">Field is required</p>
            <p class="errorText" v-if="!$v.name.minLength">Name must have at least {{ $v.name.$params.minLength.min }} figures!</p>

            <input v-model="name"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()">
          </div>
          <!-- email -->
          <div class="form-item" :class="{ errorInput: $v.email.$error}">
            <label>Email:</label>
            <p class="errorText" v-if="!$v.email.required">Field is required</p>
            <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
            <input v-model="email"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()">
          </div>
          <div class="form-item" :class="{ errorInput: $v.password.$error}">
            <label>Password:</label>
            <p class="errorText" v-if="!$v.password.required">Password is required</p>
            <p class="errorText" v-if="!$v.password.minLength">Password must have at least {{ $v.password.$params.minLength.min }} figures!!</p>
            <input v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()">
          </div>
          <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error}">
            <label>Repeat password:</label>
            <p class="errorText" v-if="!$v.repeatPassword.sameAs">Repeat password correctly!</p>
            <p class="errorText" v-if="!$v.repeatPassword.minLength">Password must have at least {{ $v.repeatPassword.$params.minLength.min }} figures!!</p>
            <input v-model="repeatPassword"
            :class="{ error: $v.repeatPassword.$error }"
            @change="$v.repeatPassword.$touch()">
          </div>
          <!-- button -->
          <button class="btn btn-primary">Submit</button>
        </form>
        <button class="to-enter" @click="toEnter">I am already registered</button>
      </div>
  </modal>
</template>

<script>
import modal from '@/components/UI/Modal.vue'
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'


export default {
  components: {
    modal
  },
  data () {
    return {
      email: '',
      name: '',
      login: '',
      password: '',
      repeatPassword: ''
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    login: {
      required,
      minLength: minLength(4)
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      sameAsPassword: sameAs('password'),
      minLength: minLength(6)
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          login: this.login,
          name: this.name,
          email: this.email,
          password: this.password
        }
        console.log(user)

        // Done
        this.login = ''
        this.name = ''
        this.email = ''
        this.password = ''
        this.repeatPassword = ''
        this.$v.$reset()
        this.$emit('close')
      }
    },
    closeModal() {
      this.login = ''
      this.name = ''
      this.email = ''
      this.password = ''
      this.repeatPassword = ''
      this.$v.$reset()
      this.$emit('close')
    },
    toEnter() {
      this.login = ''
      this.name = ''
      this.email = ''
      this.password = ''
      this.repeatPassword = ''
      this.$v.$reset()
      this.$emit('changeToEnter')
    }
  }
}
</script>

<style lang="scss">
  .form-item .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13.4px;
    color: red;
  }
  .form-item {
    &.errorInput .errorText{
    display: block;
    }
  }
  input.error {
    border-color: red;
  }
  .to-enter{
    border: none;
    font-size: 15px;
    margin-top: 17px;
  }
</style>
