<template>
  <form class="Form" id="form">
    <h3 id="validateMsg">Please fill out all the fields</h3>
    <h1 style="margin-bottom: 30px">SignUp Form</h1>

    <div class="mb-3">
      <label class="form-label">First Name</label>
      <input type="text" id="firstName" class="form-control" />
    </div>
    <div class="mb-3">
      <label class="form-label">Last Name</label>
      <input type="text" id="lastName" class="form-control" />
    </div>
    <div class="mb-3">
      <label class="form-label">Email address</label>
      <input type="email" id="email" class="form-control" />
    </div>
    <div class="mb-3 passwordContainer">
      <label class="form-label">Password</label>
      <input type="password" id="password" class="form-control" />
      <i
        class="fa-solid fa-eye passwordToggle"
        id="passwordIcon"
        @click="passwordVisibility"
      ></i>
    </div>
    <div class="mb-3">
      <label class="form-label">Address</label>
      <input type="text" id="address" class="form-control" />
    </div>
    <div class="mb-3">
      <label class="form-label">Phone</label>
      <input type="number" id="phone" class="form-control" />
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
    <div class="mb-3" id="uploadSection">
      <label class="form-label">Upload Image</label>
      <input type="file" id="Image" accept="image/*" class="form-control" />
    </div>
    <div id="imageSection" class="mb-3">
      <video
        class="form-control"
        style="margin-top: 30px"
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
        id="capturedImage"
        accept="image/*"
        style="display: none"
      />
    </div>
    <h4 id="message">Image is uploaded</h4>
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
      this.signUpData.email = document.getElementById("email").value;
      this.signUpData.firstName = document.getElementById("firstName").value;
      this.signUpData.lastName = document.getElementById("lastName").value;
      this.signUpData.phone = document.getElementById("phone").value;
      this.signUpData.Image = document.getElementById("Image").value;
      if (document.getElementById("Image").value < 1) {
        this.signUpData.Image = document.getElementById("Image").src;
      }
      this.signUpData.address = document.getElementById("address").value;
      this.signUpData.password = document.getElementById("password").value;

      if (
        this.signUpData.firstName.length < 1 ||
        this.signUpData.lastName.length < 1 ||
        this.signUpData.email.length < 1 ||
        this.signUpData.phone.length < 1 ||
        this.signUpData.Image.length < 1 ||
        this.signUpData.address.length < 1 ||
        this.signUpData.password.length < 1
      ) {
        document.getElementById("validateMsg").style.display = "block";
      } else {
        document.getElementById("validateMsg").style.display = "none";
        localStorage.setItem("signUpData", JSON.stringify(this.signUpData));
        document.getElementById("form").reset();
      }
    },

    //Password Visibility

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

    //Upload Image

    uploadHandler(e) {
      e.preventDefault();
      document.getElementById("imageSection").style.display = "none";

      document.getElementById("uploadSection").style.display = "block";
    },

    //Capture Live Image

    captureHandler(e) {
      e.preventDefault();
      document.getElementById("imageSection").style.display = "block";
      document.getElementById("uploadSection").style.display = "none";
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          const camera = document.getElementById("camera");
          camera.srcObject = stream;
          this.mediaStream = stream;
        })
        .catch(function (error) {
          console.error("Error accessing the camera: " + error);
        });
    },

    //Set Camera Image to the image tag

    captureImage(event) {
      event.preventDefault();
      const camera = document.getElementById("camera");
      const capturedImageElement = document.getElementById("Image");
      const canvas = document.createElement("canvas");
      canvas.width = 300;
      canvas.height = 300;
      const context = canvas.getContext("2d");
      context.drawImage(camera, 0, 0, canvas.width, canvas.height);

      const imageDataURL = canvas.toDataURL("image/png");
      capturedImageElement.src = imageDataURL;
      this.closeCameraStream();
      document.getElementById("imageSection").style.display = "none";
      document.getElementById("uploadSection").style.display = "none";
      document.getElementById("message").style.display = "block";
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
