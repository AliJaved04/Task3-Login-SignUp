<template>
  <form class="Form" id="form" ref="form">
    <h1 style="margin-bottom: 30px">SignUp Form</h1>

    <div class="mb-3">
      <label class="form-label">First Name</label>
      <input
        ref="firstName"
        type="text"
        v-model="signUpData.firstName"
        id="firstName"
        class="form-control"
      />
    </div>
    <div class="mb-3">
      <label class="form-label">Last Name</label>
      <input
        ref="lastName"
        type="text"
        v-model="signUpData.lastName"
        id="lastName"
        class="form-control"
      />
    </div>
    <div class="mb-3">
      <label class="form-label">Email address</label>
      <input
        type="email"
        ref="email"
        v-model="signUpData.email"
        id="email"
        class="form-control"
      />
    </div>
    <div class="mb-3 passwordContainer">
      <label class="form-label">Password</label>
      <input
        ref="password"
        type="password"
        v-model="signUpData.password"
        id="password"
        class="form-control"
      />
      <i
        class="fa-solid fa-eye passwordToggle"
        id="passwordIcon"
        @click="passwordVisibility"
      ></i>
    </div>
    <div class="mb-3">
      <label class="form-label">Address</label>
      <input
        ref="address"
        type="text"
        v-model="signUpData.address"
        id="address"
        class="form-control"
      />
    </div>
    <div class="mb-3">
      <label class="form-label">Phone</label>
      <input
        ref="phone"
        type="number"
        id="phone"
        v-model="signUpData.phone"
        class="form-control"
      />
    </div>
    <div class="mv-3">
      <button
        class="btn btn-outline-primary"
        style="margin-right: 20px"
        @click="uploadHandler"
      >
        Upload Image
      </button>
      <button class="btn btn-outline-primary" @click="captureHandler">
        Live Image
      </button>
    </div>
    <div class="mb-3" id="uploadSection" ref="uploadSection">
      <label class="form-label">Upload Image</label>
      <input
        type="file"
        id="Image"
        ref="Image"
        accept="image/*"
        class="form-control"
      />
    </div>
    <div id="imageSection" ref="imageSection" class="mb-3">
      <video
        class="form-control"
        style="margin-top: 30px"
        ref="camera"
        id="camera"
        autoplay
      ></video>
      <button
        id="captureButton"
        class="btn btn-primary"
        style="margin-top: 30px"
        @click="captureImage"
      >
        Capture Image
      </button>
      <input
        type="file"
        ref="capturedImage"
        id="capturedImage"
        accept="image/*"
        style="display: none"
      />
    </div>
    <img id="storedImage" ref="storedImage" src="../assets/logo.png" alt="" />
    <button
      type="submit"
      class="btn btn-primary"
      @click="signUpSubmit"
      style="margin-top: 20px"
    >
      Signup
    </button>

    <label class="form-label" style="margin-top: 30px"
      >Already have an account?
      <router-link class="btn btn-outline-primary" to="/">Login</router-link>
    </label>
  </form>
</template>

<script>
export default {
  name: "SignUp",

  data() {
    return {
      signUpData: {
        firstName: "",
        lastName: "",
        email: "",
        password: "",
        address: "",
        Image: "",
        phone: "",
      },

      mediaStream: {},
    };
  },
  methods: {
    //SignUp Submit
    signUpSubmit(event) {
      event.preventDefault();
      this.signUpData.Image = this.$refs.Image.value;
      if (this.signUpData.Image.length < 1) {
        this.signUpData.Image = this.$refs.storedImage.src;
      }

      if (
        this.signUpData.firstName.length < 1 ||
        this.signUpData.lastName.length < 1 ||
        this.signUpData.email.length < 1 ||
        this.signUpData.phone.length < 1 ||
        this.signUpData.Image.length < 1 ||
        this.signUpData.address.length < 1 ||
        this.signUpData.password.length < 1
      ) {
      } else {
        localStorage.setItem("signUpData", JSON.stringify(this.signUpData));
        this.$refs.form.reset();
      }
    },

    //Password Visibility

    passwordVisibility() {
      if (this.$refs.password.type === "password") {
        this.$refs.password.type = "text";
        this.$refs.passwordIcon.classList.remove("fa-eye");
        this.$refs.passwordIcon.classList.add("fa-eye-slash");
      } else {
        this.$refs.passwordIcon.classList.add("fa-eye");
        this.$refs.passwordIcon.classList.remove("fa-eye-slash");
        this.$refs.password.type = "password";
      }
    },

    //Upload Image

    uploadHandler(e) {
      e.preventDefault();
      this.$refs.imageSection.style.display = "none";

      this.$refs.uploadSection.style.display = "block";
    },

    //Capture Live Image

    captureHandler(e) {
      e.preventDefault();
      this.$refs.imageSection.style.display = "block";
      this.$refs.uploadSection.style.display = "none";
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          // const camera = document.getElementById("camera");
          this.$refs.camera.srcObject = stream;
          this.mediaStream = stream;
        })
        .catch(function (error) {
          console.error("Error accessing the camera: " + error);
        });
    },

    //Set Camera Image to the image tag

    captureImage(event) {
      event.preventDefault();

      const camera = this.$refs.camera;
      const capturedImageElement = this.$refs.Image;
      const canvas = document.createElement("canvas");
      canvas.width = 300;
      canvas.height = 300;
      const context = canvas.getContext("2d");
      context.drawImage(camera, 0, 0, canvas.width, canvas.height);

      const imageDataURL = canvas.toDataURL("image/png");
      this.$refs.storedImage.src = imageDataURL;
      this.$refs.storedImage.style.display = "block";
      capturedImageElement.value = "";
      this.$refs.imageSection.style.display = "none";
      this.$refs.uploadSection.style.display = "none";
      this.closeCameraStream();
    },

    //Close Camera Stream

    closeCameraStream() {
      if (this.mediaStream) {
        this.mediaStream.getTracks().forEach(function (track) {
          track.stop();
        });
      }
    },
  },
};
</script>
