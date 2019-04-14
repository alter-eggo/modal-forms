<template lang="html">
  <modal title="Enter form"
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
          <div class="form-item" :class="{ errorInput: $v.password.$error}">
            <label>Password:</label>
            <p class="errorText" v-if="!$v.password.required">Password is required</p>
            <p class="errorText" v-if="!$v.password.minLength">Password must have at least {{ $v.password.$params.minLength.min }} figures!!</p>
            <input v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()">
          </div>
          <!-- button -->
          <button class="btn btn-primary">Submit</button>
        </form>
        <button class="to-enter" @click="toRegistration">I need registration</button>
      </div>
  </modal>
</template>

<script>
import modal from '@/components/UI/Modal.vue'
import { required, minLength } from 'vuelidate/lib/validators'


export default {
  components: {
    modal
  },
  data () {
    return {
      login: '',
      password: '',
    }
  },
  validations: {
    login: {
      required,
      minLength: minLength(4)
    },
    password: {
      required,
      minLength: minLength(6)
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          login: this.login,
          password: this.password
        }
        console.log(user)

        // Done
        this.login = ''
        this.password = ''
        this.$v.$reset()
        this.$emit('close')
      }
    },
    closeModal() {
      this.login = ''
      this.password = ''
      this.$v.$reset()
      this.$emit('close')
    },
    toRegistration() {
      this.login = ''
      this.password = ''
      this.$v.$reset()
      this.$emit('changeToRegistration')
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
</style>
