<template>
  <div class="form-container">
    <div class="content-container">
      <div class="column" v-if="!submitted">
        <!-- Image for GreenTech Logo -->
        <img
          :src="require('@/assets/image.png')"
          alt="GreenTech Logo"
          class="logo"
        />
        <!-- Title -->
        <h1 class="form-title">Have us reach out</h1>

        <!-- Starting form -->
        <!-- Handles the First name -->
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label class="form-label">First Name</label>
            <input
              type="text"
              v-model="form.first"
              class="form-input"
              required
            />
          </div>
          <!-- Handles Last Name -->
          <div class="form-group">
            <label class="form-label">Last Name</label>
            <input
              type="text"
              v-model="form.last"
              class="form-input"
              required
            />
          </div>
          <!-- Handles the email -->
          <div class="form-group">
            <label class="form-label">Email</label>
            <input
              type="email"
              v-model="form.email"
              class="form-input"
              required
            />
          </div>
          <!-- Handles Phone number -->
          <div class="form-group">
            <label class="form-label">Phone Number</label>
            <input
              type="tel"
              v-model="form.phone"
              class="form-input"
              required
            />
          </div>
          <!-- Handles company -->
          <div class="form-group">
            <label class="form-label">Company</label>
            <input
              type="text"
              v-model="form.company"
              class="form-input"
              required
            />
          </div>
          <!-- Error messagae in case of mistakes -->
          <div v-if="errorMessage" class="error-message">
            {{ errorMessage }}
          </div>
          <!-- Submit button container -->
          <div class="button-container">
            <button
              type="submit"
              class="submit-button"
              :disabled="isSubmitting"
            >
              {{ isSubmitting ? "Submitting..." : "Continue" }}
            </button>
          </div>
        </form>
      </div>

      <!-- Thank You Section after submitting -->
      <div class="thank-you-column" v-if="submitted">
        <img
          :src="require('@/assets/image.png')"
          alt="GreenTech Logo"
          class="logo"
        />
        <div class="thank-you-content">
          <h2 class="thank-you-title">Thank you</h2>
          <p class="thank-you-message">We will contact you shortly</p>
        </div>
      </div>
    </div>
  </div>
</template>
<!-- Scripting for API -->
<script>
// Layout for the forms inputs so that the API can take the info and put it into the database
export default {
  name: "ContactForm",
  data() {
    return {
      form: {
        first: "",
        last: "",
        company: "",
        phone: "",
        email: "",
      },
      submitted: false,
      isSubmitting: false,
      errorMessage: "",
    };
  }, // Submit method handling for when the form is completed
  methods: {
    async handleSubmit() {
      this.isSubmitting = true;
      this.errorMessage = "";

      const cleanedForm = {
        ...this.form,
        phone: this.form.phone.replace(/\D/g, ""), // digits only
      };

      console.log("Submitting:", JSON.stringify(cleanedForm, null, 2));

      try {
        const res = await fetch(
          `https://dev-api-api.hiring-test.experientialpreview.com/api/lead/7bd619ae-81e8-4204-914d-6643edde5c2e`,
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(cleanedForm),
          }
        );

        if (!res.ok) {
          const errorData = await res.json().catch(() => null);
          console.error("API Response Error:", errorData);
          throw new Error(
            errorData?.message ||
              JSON.stringify(errorData) ||
              `Failed to submit (Status ${res.status})`
          );
        }

        this.submitted = true;
        setTimeout(() => this.resetForm(), 5000);
      } catch (error) {
        console.error("Error submitting form:", error);
        this.errorMessage =
          error.message ||
          "There was an error submitting the form. Please try again.";
      } finally {
        this.isSubmitting = false;
      }
    }, // Form reset procedure for when the timeout goes back to the form
    resetForm() {
      this.form = {
        first: "",
        last: "",
        company: "",
        phone: "",
        email: "",
      };
      this.submitted = false;
      this.errorMessage = "";
    },
  },
};
</script>
<!-- Styling put into CSS for better readablity -->
<style src="./style.css"></style>
