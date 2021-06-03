<template>
  <modal title="Modal with form + Validate" @close="$emit('close')">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <input
            v-model="name"
            class="form-item-input"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()"
          />
          <p class="errorText" v-if="!$v.name.required && $v.email.$dirty">
            Field is required!
          </p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name must have at least {{ $v.name.$params.minLength.min }} !
          </p>
        </div>

        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <input
            v-model="email"
            class="form-item-input"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()"
          />
          <p v-if="!$v.email.required && $v.email.$dirty" class="errorText">
            Field is required!
          </p>
          <p v-if="!$v.email.email" class="errorText">Email is not correct!</p>
        </div>
        <button class="btn btnPrimary">Submit!</button>
      </form>
    </div>
  </modal>
</template>



<script>
import { required, minLength, email } from "vuelidate/lib/validators";

import modal from "@/components/UI/Modal.vue";

export default {
  components: {
    modal,
  },
  created() {
    this.$v.$reset();
  },
  data() {
    return {
      email: "",
      name: "",
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4),
    },
    email: {
      required,
      email,
    },
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
        };
        console.log(user);
        this.name = "";
        this.email = "";
        this.$v.$reset();
        this.$emit("close");
      }
    },
    handleEmailRequired() {
      return !$v.email.required;
    },
  },
};
</script>


<style lang="scss">
.form-item .errorText {
  margin-bottom: 8px;
  font-size: 13.4px;
  color: rgb(243, 92, 92);
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}
.form-item {
  &.errorInput.errorText {
    display: block;
  }
}
.form-item-input {
  margin-bottom: 10px;
}
input.error {
  border-color: rgb(243, 92, 92);
}
</style>>