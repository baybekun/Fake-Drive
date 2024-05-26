<template>
  <div class="Wrap">
    <div class="left">
      <div class="logof">
        <img src="/image/logo.png" />
      </div>

      <div class="text">
        <div class="loginTitle" align="center">Đăng nhập</div>
        <div class="subLoginTitle" align="center">
          Đăng nhập vào<br />
          tài khoản của bạn
        </div>
      </div>
    </div>
    <div class="right">
      <div class="sub-content">
        <div class="supercloud" align="center">SuperCloud</div>

        <Form @submit="handleLogin" :validation-schema="schema">
          <div class="textfield" name="email">
            <Field
              name="txtEmail"
              type="text"
              class="EmailText"
              placeholder="Email"
            />
            <ErrorMessage name="txtEmail" class="error-feedback" />
          </div>

          <div class="textfield" name="password">
            <Field
              name="txtPassword"
              type="text"
              class="PasswordText"
              placeholder="Password"
            />
            <ErrorMessage name="txtPassword" class="error-feedback" />
          </div>

          <div class="forgot_pass">
            <a href="/forgetpassword">Quên Mật Khẩu?</a>
          </div>
          <div class="non_acc">
            <a href="/register">Chưa có tài khoản?</a>
          </div>
          <div class="button">
            <button
              class="NextButton"
              type="submit"
              name="butNext"
              :disabled="loading"
            >
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              <span>Login</span>
            </button>
          </div>

          <div class="form-group">
            <div v-if="message" class="alert alert-danger" role="alert">
              {{ message }}
            </div>
          </div>
        </Form>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "logins",
});
</script>
<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";

export default {
  name: "logins",
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    const schema = yup.object().shape({
      username: yup.string().required("Username is required!"),
      password: yup.string().required("Password is required!"),
    });

    return {
      loading: false,
      message: "",
      schema,
    };
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    },
  },
  created() {
    if (this.loggedIn) {
      this.$router.push("/");
    }
  },
  methods: {
    handleLogin(user) {
      this.loading = true;

      this.$store.dispatch("auth/login", user).then(
        () => {
          this.$router.push("/");
        },
        (error) => {
          this.loading = false;
          this.message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();
        }
      );
    },
  },
};
</script>

<style scoped>
.Wrap {
  background-color: white;
  width: 1254px;
  height: 528px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 30px;
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.2);
}
.left {
  float: left;
  width: 332px;
  height: 477px;
  background-color: white;
  margin-top: 25px;
  margin-left: 20px;
  border-radius: 30px;
}
.right {
  float: right;
  width: 839px;
  height: 477px;
  background-color: #efefef;
  margin-top: 25px;
  margin-right: 49px;
  border-radius: 30px;
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1); /* Tạo hiệu ứng đổ bóng */
}
.logo {
  margin-left: auto;
  background-color: white;
  border-radius: 30px;
}

.loginTitle {
  font-size: 62px; /* Đặt kích thước font chữ */
  font-family: "Inria Serif", serif; /* Sử dụng font Inria Serif */
  padding-bottom: 20px;
}
.subLoginTitle {
  font-size: 32px;
  font-family: "Inria Serif", serif; /* Sử dụng font Inria Serif */
  color: #696969;
  float: left;
  margin-left: 45px;
}

.supercloud {
  font-size: 68px; /* Đặt kích thước font chữ */
  font-family: "Inria Serif"; /* Sử dụng font Inria Serif */
  margin-top: 10px;
  background: linear-gradient(
    to bottom,
    #697b83,
    #062537
  ); /* Gradient màu từ trắng đến #062537 từ trên xuống dưới */
  -webkit-background-clip: text; /* Cho phép áp dụng background cho text */
  -webkit-text-fill-color: transparent; /* Đặt màu chữ trong suốt */
}
.textfiled input {
  background-color: aqua;
  border-radius: 30px;
  border: none;
  outline: none;
}
.EmailText {
  margin-left: 43px;
  margin-top: 20px;
  width: 744px;
  height: 96px;
  border: none;
  border-radius: 30px;
  font-size: 32px;
  padding-left: 10px;
}
.EmailText::placeholder {
  font-size: 32px;
  font-family: "Inria Serif";
  padding-left: 10px;
}
.PasswordText {
  margin-left: 43px;
  margin-top: 30px;
  width: 744px;
  height: 96px;
  border: none;
  border-radius: 30px;
  font-size: 32px;
  padding-left: 10px;
}
.PasswordText::placeholder {
  font-size: 32px;
  font-family: "Inria Serif";
  padding-left: 10px;
}
.forgot_pass {
  font-size: 24px;
  font-family: "Inria Serif";
  margin-top: 20px;
  margin-left: 65px;
  color: #062537;
}
.non_acc {
  font-size: 24px;
  font-family: "Inria Serif";
  margin-top: -36px;
  margin-left: 259px;
  color: #062537;
  width: fit-content;
  height: fit-content;
  float: left;
}
.button {
  width: fit-content;
  height: fit-content;
}
.NextButton {
  width: 157px;
  height: 62px;
  border-radius: 30px;
  border: none;
  background-color: #062537;
  color: white;
  font-family: "Inria Serif";
  font-size: 24px;
  margin-right: 50px;
  margin-left: 626px;
  margin-top: -15px;
  float: right;
}
</style>
<style>
.logof {
  width: 264px;
  height: 208px;
  margin-top: 32px;
}
</style>
