<template>
  <div class="login-container">
    <div class="login-box">
      <h3 class="login-header">Log In</h3>
      <form @submit.prevent="LogIn">
        <div class="input-group">
          <label for="username">User Name</label>
          <input
            type="text"
            id="username"
            name="username"
            required
            v-model="username"
          />
        </div>
        <div class="input-group">
          <label for="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            required
            v-model="password"
          />
        </div>
        <div class="button-group">
          <button type="submit" class="btn btn-login">Log In</button>
          <button
            type="button"
            @click="this.$router.push('/signup')"
            class="btn btn-signup"
          >
            Sign Up
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { jwtDecode } from "jwt-decode";

export default {
  name: "LogIn",
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    async LogIn() {
      const data = {
        name: this.username,
        password: this.password,
      };

      await fetch("http://localhost:8089/api/auth/login", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      })
        .then((response) => response.text())
        .then((token) => {
          if (token.startsWith("ey")) {
            this.decodedToken = jwtDecode(token);
            this.roles = this.decodedToken.roles;
            localStorage.setItem("jwtToken", token);
            this.$router.push("/");
          }
        });
    },
  },
};
</script>
<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(150deg, #1abc9c 50%, #3498db 50%);
}

.login-box {
  background: white;
  padding: 40px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}

.login-header {
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 20px;
  text-align: center;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
}

.input-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.button-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn {
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  font-size: 16px;
}

.btn-login {
  background-color: #1877f2;
  color: white;
  flex: 1;
  margin-right: 10px;
}

.btn-login:hover {
  background-color: #145dbf;
}

.btn-signup {
  background-color: #42b72a;
  color: white;
  flex: 1;
  margin-left: 10px;
}

.btn-signup:hover {
  background-color: #36a420;
}
</style>
