<template>
  <ValidationObserver ref="observer" v-slot="{ handleSubmit }">
    <section class="section">
      <BInputWithValidation rules="required" type="text" label="Name" v-model="form.name"/>
      <BInputWithValidation rules="required|email" type="email" label="Email" v-model="form.email"/>

      <div class="buttons">
        <button class="button is-success" @click="handleSubmit(submit)">
          <span>Submit</span>
        </button>

        <button class="button" @click="resetForm">
          <span>Reset</span>
        </button>
      </div>
    </section>
  </ValidationObserver>
</template>

<script>
import { ValidationObserver } from "vee-validate"
import BInputWithValidation from "./inputs/BInputWithValidation"

export default {
  name: "Form",
  components: {
    ValidationObserver,
    BInputWithValidation
  },
  data: () => ({
    form: {
      name: "",
      email: ""
    }
  }),
  methods: {
    async submit() {
      const args = {
        url: '/requests/create',
        method: 'POST',
        data: this.form
      }

      const response = await this.$axios(args)

      if (response.data.status === 'success') {
        this.resetForm()

        alert('Success')
      }

      console.log(response)
    },
    resetForm() {
      this.form.name = ""
      this.form.email = ""

      requestAnimationFrame(() => {
        this.$refs.observer.reset()
      })
    }
  }
}
</script>
