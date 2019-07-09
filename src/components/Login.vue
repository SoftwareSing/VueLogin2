<template>
  <div class="login">
    <h3>Sign In</h3>
    <el-card class="login_card">
      <div>
        <el-input class ="input" type="text" v-model="email" placeholder="Email" clearable></el-input>
      </div>
      <div>
        <el-input class ="input" type="password" v-model="password" placeholder="Password" clearable></el-input>
      </div>
      <el-button class ="button" type="primary" v-on:click="signIn">Connection</el-button>
    </el-card>
  </div>
</template>

<script>
import firebase from "firebase";
import { Input, Button, Card, Col, Row } from "element-ui";
export default {
  name: "login",
  data: function() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    signIn: function() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(
          user => {
            localStorage.setItem("token", "ImLogin");
            this.$router.push("/dashboard");
          },
          err => {
            alert("Oops. " + err.message);
          }
        );
    }
  },
  components: {
    "el-input": Input,
    "el-button": Button,
    "el-card": Card,
    "el-col": Col,
    "el-row": Row
  }
};
</script>

<style scoped>
/* "scoped" attribute limit the CSS to this component only */
@import url("element-ui/lib/theme-chalk/index.css");
.login {
  margin-top: 40px;
}
.input {
  margin: 10px 0;
  width: 260px;
}
.button {
  margin-top: 20px;
  width: 260px;
}
.login_card {
  margin: 0 auto; /* Added */
  float: none; /* Added */
  margin-bottom: 10px; /* Added */
  width: 450px;
  background: #DCDFE6;
}
p {
  margin-top: 40px;
  font-size: 13px;
}
p a {
  text-decoration: underline;
  cursor: pointer;
}
</style>