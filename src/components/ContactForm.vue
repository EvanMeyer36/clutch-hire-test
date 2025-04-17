<template>
  <div class="form-container">
    <div class="content-container">
      <!-- Form Column -->
      <div class="column" v-if="!submitted">
        <img src="/src/assets/image.png" alt="GreenTech Logo" class="logo" />

        <h1 class="form-title">Have us reach out</h1>

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

          <div class="form-group">
            <label class="form-label">Last Name</label>
            <input
              type="text"
              v-model="form.last"
              class="form-input"
              required
            />
          </div>

          <div class="form-group">
            <label class="form-label">Email</label>
            <input
              type="email"
              v-model="form.email"
              class="form-input"
              required
            />
          </div>

          <div class="form-group">
            <label class="form-label">Phone Number</label>
            <input
              type="tel"
              v-model="form.phone"
              class="form-input"
              required
            />
          </div>

          <div class="form-group">
            <label class="form-label">Company</label>
            <input
              type="text"
              v-model="form.company"
              class="form-input"
              required
            />
          </div>

          <div v-if="errorMessage" class="error-message">
            {{ errorMessage }}
          </div>

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

      <!-- Thank You Column -->
      <div class="column thank-you-column" v-if="submitted">
        <img src="/logo.svg" alt="GreenTech Logo" class="logo" />

        <div class="thank-you-content">
          <h2 class="thank-you-title">Thank you</h2>
          <p class="thank-you-message">We will contact you shortly</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
  },
  methods: {
    async handleSubmit() {
      this.isSubmitting = true;
      this.errorMessage = "";

      try {
        const res = await fetch(
          `https://dev-api-api.hiring-test.experientialpreview.com/api/lead/7bd619ae-81e8-4204-914d-6643edde5c2e`,
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(this.form),
          }
        );

        if (!res.ok) {
          const errorData = await res.json().catch(() => null);
          throw new Error(
            errorData?.message || `Failed to submit (Status ${res.status})`
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
    },
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

<style scoped>
.form-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}

.content-container {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.column {
  flex: 1;
  background-color: #fff;
  border-radius: 8px;
  padding: 40px;
  max-width: 450px;
}

.logo {
  width: 180px;
  margin-bottom: 40px;
}

.form-title {
  font-size: 28px;
  margin-bottom: 30px;
  color: #444;
  font-family: "Roboto", sans-serif;
  font-weight: normal;
}

.form-group {
  margin-bottom: 20px;
}

.form-label {
  display: block;
  color: #2e7d32;
  font-size: 14px;
  margin-bottom: 8px;
  font-family: "Roboto Slab", serif;
}

.form-input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.button-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 30px;
}

.submit-button {
  background-color: #1a4971;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  width: 100%;
  max-width: 150px;
}

.submit-button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.error-message {
  color: #e74c3c;
  margin-top: 10px;
  font-size: 14px;
}

.thank-you-column {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.thank-you-content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.thank-you-title {
  font-size: 32px;
  margin-bottom: 20px;
  color: #444;
  font-weight: normal;
}

.thank-you-message {
  font-size: 20px;
  color: #666;
}
</style>
