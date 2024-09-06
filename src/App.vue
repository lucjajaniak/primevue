<script setup>
import { ref, computed } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, minLength, email } from '@vuelidate/validators';

// Form fields
const name = ref('');
const mail = ref('');
const pass = ref('');

// Validation rules
const rules = computed(() => ({
  name: {
    required,
    minLength: minLength(3),
  },
  mail: {
    required,
    email, // Validates the email format
  },
  pass: {
    required,
    minLength: minLength(6), // Password must be at least 6 characters long
  },
}));

// Vuelidate instance
const $v = useVuelidate(rules, { name, mail, pass });

// Form submission handler
const submitForm = async () => {
  const isValid = await $v.value.$validate(); // Validates all fields
  console.log($v);
  if (isValid) {
    alert('Form submitted successfully.');
  } else {
    alert('Form has validation errors.');
  }
};
</script>

<template>
  <form @submit.prevent="submitForm">
    <!-- Username -->
    <FloatLabel>
      <InputText
        :invalid="$v.name.$invalid"
        id="username"
        v-model="name"
        aria-describedby="name-help"
      />
      <label for="username">Username</label>
    </FloatLabel>
    <small v-if="$v.name.$error">
      <span v-if="!$v.name.required">Name is required.</span>
      <span v-if="!$v.name.minLength">Name must be at least 3 characters long.</span>
    </small>
    <br>

    <!-- Email -->
    <FloatLabel>
      <InputText
        :invalid="$v.mail.$invalid"
        id="email"
        v-model="mail"
        aria-describedby="mail-help"
      />
      <label for="email">E-mail</label>
    </FloatLabel>
    <small v-if="$v.mail.$error">
      <span v-if="!$v.mail.required">Email is required.</span>
      <span v-if="!$v.mail.email">Email must be valid.</span>
    </small>
    <br>

    <!-- Password -->
    <FloatLabel>
      <Password
        :invalid="$v.pass.$invalid"
        v-model="pass"
        inputId="password"
        aria-describedby="pass-help"
      />
      <label for="password">Password</label>
    </FloatLabel>
    <small v-if="$v.pass.$error">
      <span v-if="!$v.pass.required">Password is required.</span>
      <span v-if="!$v.pass.minLength">Password must be at least 6 characters long.</span>
    </small>
    <br>

    <!-- Submit button -->
    <Button type="submit" label="Submit" />
  </form>
</template>

<style scoped>
/* Add custom styling if needed */
</style>
