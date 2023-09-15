<template>
  <form class="Form" id="form" ref="form">
    <h1 style="margin-bottom: 30px">Login Form</h1>
    <div class="mb-3">
      <label class="form-label">Email address</label>
      <input
        type="email"
        id="email"
        ref="emailField"
        v-model="loginData.email"
        class="form-control"
      />
    </div>
    <div class="mb-3 passwordContainer">
      <label class="form-label">Password</label>
      <input
        type="password"
        ref="passwordField"
        v-model="loginData.password"
        id="password"
        class="form-control"
      />
      <i
        class="fa-solid fa-eye passwordToggle"
        id="passwordIcon"
        ref="passwordIcon"
        @click="passwordVisibility"
      ></i>
    </div>
    <button type="submit" class="btn btn-primary" @click="loginSubmit">
      Login
    </button>

    <label class="form-label" style="margin-top: 30px"
      >Do not have an account?
      <router-link class="btn btn-outline-primary" to="/signup"
        >SignUp</router-link
      >
    </label>
  </form>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      loginData: {
        email: "",
        password: "",
      },

      mediaStream: {},
    };
  },

  onMounted() {
    console.log("object");
  },

  methods: {
    //Login Submit

    loginSubmit(event) {
      event.preventDefault();
      if (this.validate()) {
        localStorage.setItem("loginData", JSON.stringify(this.loginData));
        this.$refs.form.reset();
      }
    },

    validate() {
      if (
        this.$refs.emailField.value.length < 1 ||
        this.$refs.passwordField.value.length < 1
      ) {
        if (
          this.$refs.emailField.value.length < 1 &&
          this.$refs.passwordField.value.length < 1
        ) {
          this.$toast.error("Please Enter Email and Password", {
            position: "top-right",
          });
        } else if (this.$refs.emailField.value.length < 1) {
          this.$toast.error("Please Enter Email", {
            position: "top-right",
          });
        } else if (this.$refs.passwordField.value.length < 1) {
          this.$toast.error("Please Enter Password", {
            position: "top-right",
          });
        }
        return false;
      }
      return true;
    },

    //Password Icon

    passwordVisibility() {
      if (this.$refs.passwordField.type === "password") {
        this.$refs.passwordField.type = "text";
        this.$refs.passwordIcon.classList.remove("fa-eye");
        this.$refs.passwordIcon.classList.add("fa-eye-slash");
      } else {
        this.$refs.passwordIcon.classList.add("fa-eye");
        this.$refs.passwordIcon.classList.remove("fa-eye-slash");
        this.$refs.passwordField.type = "password";
      }
    },
  },
};
</script>
