<template>
  <div class="card card-signin my-5">
    <div class="card-body">
      <form class="form-signin" @submit.prevent="login">
        <div class="form-label-group">
          <input
            type="email"
            id="inputEmail"
            class="form-control"
            placeholder="Email address"
            v-model="email"
            autofocus
          />
          <label for="inputEmail">Email address</label>
        </div>

        <div class="form-label-group">
          <input
            type="password"
            id="inputPassword"
            class="form-control"
            placeholder="Password"
            v-model="password"
          />
          <label for="inputPassword">Password</label>
        </div>
        <button
          class="btn btn-lg btn-primary btn-block text-uppercase"
          type="submit"
        >
          Log In
          <i class="fas fa-sign-in-alt"></i>
        </button>
        <hr class="my-4" />
        <a href="javascript:;" @click="switchToggle">First time here?</a>
      </form>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";
const Toast = Swal.mixin({
  toast: true,
  position: "bottom-end",
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  onOpen: toast => {
    toast.addEventListener("mouseenter", Swal.stopTimer);
    toast.addEventListener("mouseleave", Swal.resumeTimer);
  }
});

export default {
  name: "LandingLogin",
  data() {
    return {
      email: null,
      password: null
    };
  },
  methods: {
    switchToggle() {
      this.$emit("switch-toggle");
    },
    login() {
      this.$store
        .dispatch("login", {
          email: this.email,
          password: this.password
        })
        .then(({ data }) => {
          Toast.fire({
            icon: "success",
            title: data.msg
          });
          this.$store.commit("SET_USER", {
            email: this.email,
            username: data.data.username,
            role: data.data.role
          });
          localStorage.setItem("token", data.data.token);
          this.$store.commit("SET_LOGIN", true);
          this.$router.push("/carts");
          this.email = null;
          this.password = null;
        })
        .catch(({ response }) => {
          Toast.fire({
            icon: "error",
            title: response.data.msg
          });
        })
        .finally(() => {
          this.$store.commit("SET_LOADING", false);
        });
    }
  }
};
</script>

<style scoped>
card-signin {
  border: 0;
  border-radius: 1rem;
  box-shadow: 0 0.5rem 1rem 0 rgba(0, 0, 0, 0.1);
}

.card-signin .card-title {
  margin-bottom: 2rem;
  font-weight: 300;
  font-size: 1.5rem;
}

.card-signin .card-body {
  padding: 2rem;
}

.form-signin {
  width: 100%;
}

.form-signin .btn {
  font-size: 80%;
  border-radius: 5rem;
  letter-spacing: 0.1rem;
  font-weight: bold;
  padding: 1rem;
  transition: all 0.2s;
}

.form-label-group {
  position: relative;
  margin-bottom: 1rem;
}

.form-label-group input {
  height: auto;
  border-radius: 2rem;
}

.form-label-group > input,
.form-label-group > label {
  padding: var(--input-padding-y) var(--input-padding-x);
}

.form-label-group > label {
  position: absolute;
  top: 0;
  left: 0;
  display: block;
  width: 100%;
  margin-bottom: 0;
  /* Override default `<label>` margin */
  line-height: 1.5;
  color: #495057;
  border: 1px solid transparent;
  border-radius: 0.25rem;
  transition: all 0.1s ease-in-out;
}

.form-label-group input::-webkit-input-placeholder {
  color: transparent;
}

.form-label-group input:-ms-input-placeholder {
  color: transparent;
}

.form-label-group input::-ms-input-placeholder {
  color: transparent;
}

.form-label-group input::-moz-placeholder {
  color: transparent;
}

.form-label-group input::placeholder {
  color: transparent;
}

.form-label-group input:not(:placeholder-shown) {
  padding-top: calc(var(--input-padding-y) + var(--input-padding-y) * (2 / 3));
  padding-bottom: calc(var(--input-padding-y) / 3);
}

.form-label-group input:not(:placeholder-shown) ~ label {
  padding-top: calc(var(--input-padding-y) / 3);
  padding-bottom: calc(var(--input-padding-y) / 3);
  font-size: 12px;
  color: #777;
}

.btn-google {
  color: white;
  background-color: #ea4335;
}

.btn-facebook {
  color: white;
  background-color: #3b5998;
}

:root {
  --input-padding-x: 1.5rem;
  --input-padding-y: 0.75rem;
}
</style>
