<script>
// Defult to the ContactForm.vue file to start the form
import ContactForm from "./components/ContactForm.vue";

export default {
  name: "App",
  components: {
    ContactForm,
  },
};
</script>

<template>
  <div id="app">
    <ContactForm />
  </div>
</template>
