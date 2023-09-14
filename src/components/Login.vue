<template>
  <form class="Form" id="form">
    <h3 id="validateMsg">Please fill out all the fields</h3>
    <h1 style="margin-bottom: 30px">Login Form</h1>
    <div class="mb-3">
      <label class="form-label">Email address</label>
      <input
        type="email"
        id="email"
        v-model="loginData.email"
        class="form-control"
      />
    </div>
    <div class="mb-3 passwordContainer">
      <label class="form-label">Password</label>
      <input
        type="password"
        v-model="loginData.password"
        id="password"
        class="form-control"
      />
      <i
        class="fa-solid fa-eye passwordToggle"
        id="passwordIcon"
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

      if (
        this.loginData.email.length < 1 ||
        this.loginData.password.length < 1
      ) {
        document.getElementById("validateMsg").style.display = "block";
      } else {
        document.getElementById("validateMsg").style.display = "none";
        localStorage.setItem("loginData", JSON.stringify(this.loginData));
        document.getElementById("form").reset();
      }
    },

    //Password Icon

    passwordVisibility() {
      const password = document.getElementById("password");
      const passwordIcon = document.getElementById("passwordIcon");

      if (password.type === "password") {
        password.type = "text";
        passwordIcon.classList.remove("fa-eye");
        passwordIcon.classList.add("fa-eye-slash");
      } else {
        passwordIcon.classList.add("fa-eye");
        passwordIcon.classList.remove("fa-eye-slash");
        password.type = "password";
      }
    },
  },
};
</script>
