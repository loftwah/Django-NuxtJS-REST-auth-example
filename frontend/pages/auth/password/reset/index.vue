<template>
  <v-container>
    <v-layout justify-center>
      <v-card>
        <v-card-text>
          <v-container>
            <form @submit.prevent="onRestorePassword">
              <v-layout row>
                <v-flex xs-12>
                  <v-text-field
                    id="email"
                    v-model="email"
                    :rules="emailRules"
                    :error="errors.email.length > 0"
                    :error-messages="errors.email"
                    name="email"
                    label="Email"
                    type="email"
                    @focus="errors.email = []"
                  />
                </v-flex>
              </v-layout>
              <v-layout justify-center mt-3>
                <v-btn
                  :disabled="loading"
                  :loading="loading"
                  type="submit"
                  color="success"
                  class="grey--text text--darken-4"
                >
                  Restore
                  <span slot="loader" class="custom-loader">
                    <v-icon light>refresh</v-icon>
                  </span>
                </v-btn>
              </v-layout>
            </form>
          </v-container>
        </v-card-text>
      </v-card>
    </v-layout>
  </v-container>
</template>
<style>
@import '@/assets/style/custom-loader.css';
</style>
<script>
export default {
  head: {
    title: 'Password reset page'
  },
  data() {
    return {
      email: '',
      emailRules: [value => !!value || 'Required.'],
      errors: {
        email: []
      }
    }
  },
  computed: {
    error() {
      return this.$store.getters.error
    },
    loading() {
      return this.$store.getters.loading
    }
  },
  watch: {
    error(values) {
      if (values !== null) {
        for (const value in values.response.data) {
          this.errors[value] = values.response.data[value]
        }
      }
    }
  },
  methods: {
    onRestorePassword() {
      this.$store.dispatch('auth/restorePassword', {
        email: this.email
      })
    }
  }
}
</script>
