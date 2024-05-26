<template>
  <div class="Wrap">
    <div class="left">
      <div class="logof">
        <img src="/image/logo.png" />
      </div>

      <div class="text">
        <div class="SignInTitle" align="center">Đăng Kí</div>
        <div class="subSignInTitle" align="center">
          <Table></Table>Tạo mới tài khoản<br />
          để sử dụng <br />
          SuperCloud
        </div>
      </div>
    </div>
    <div class="right">
      <div class="sub-content">
        <div class="supercloud" align="center">SuperCloud</div>
        <Form @submit="handleRegister" :validation-schema="schema">
          <div class="textfield" name="Name">
            <Field
              class="EmailText"
              type="text"
              name="txtEmail"
              placeholder="Họ và Tên"
            />
          </div>
          <div class="textfield" name="Email">
            <Field
              class="PasswordText"
              type="text"
              name="txtPassword"
              placeholder="Email"
            />
          </div>
          <div class="textfield" name="Username">
            <Field
              class="PasswordText"
              type="text"
              name="txtPassword"
              placeholder="Username"
            />
          </div>
          <div class="textfield" name="Password">
            <Field
              class="PasswordText"
              type="text"
              name="txtPassword"
              placeholder="Password"
            />
          </div>
          <div class="non_acc"><a href="/logins">Đã có tài khoản?</a></div>
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
              Sign Up
            </button>
          </div>

          <div
            v-if="message"
            class="alert"
            :class="successful ? 'alert-success' : 'alert-danger'"
          >
            {{ message }}
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
  name: "register",
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    const schema = yup.object().shape({
      username: yup
        .string()
        .required("Yêu cầu nhập tên người dùng")
        .min(3, "Ít nhất có 3 kí tự")
        .max(20, "Nhiều nhất 20 kí tự"),
      email: yup
        .string()
        .required("Yêu cầu nhập email")
        .email("Email không hợp lệ!")
        .max(50, "Tối đa không quá 50 kí tự"),
      password: yup
        .string()
        .required("Nhập mật khẩu")
        .min(6, "Ít nhất có 6 kí tự")
        .max(40, "Nhiều nhất 40 kí tự"),
    });

    return {
      successful: false,
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
  mounted() {
    if (this.loggedIn) {
      this.$router.push("/");
    }
  },
  methods: {
    handleRegister(user) {
      this.message = "";
      this.successful = false;
      this.loading = true;

      this.$store.dispatch("auth/register", user).then(
        (data) => {
          this.message = data.message;
          this.successful = true;
          this.loading = false;
        },
        (error) => {
          this.message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();
          this.successful = false;
          this.loading = false;
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
  height: 844px;
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
  height: 759px;
  background-color: #efefef;
  margin-top: 40px;
  margin-right: 49px;
  border-radius: 30px;
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1);
}
.logof {
  background-color: white;
  border-radius: 30px;
}

.SignInTitle {
  font-size: 62px;
  font-family: "Inria Serif", serif;
}
.subSignInTitle {
  font-size: 32px;
  font-family: "Inria Serif", serif;
  color: #696969;
  float: left;
  margin-left: 45px;
  margin-top: 15px;
}

.supercloud {
  font-size: 68px;
  font-family: "Inria Serif";
  margin-top: 10px;
  background: linear-gradient(to bottom, #697b83, #062537);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
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

.non_acc {
  font-size: 24px;
  font-family: "Inria Serif";
  margin-top: 20px;
  margin-left: 380px;
  color: #062537;
  width: fit-content;
  height: fit-content;
  float: left;
  margin-top: 70px;
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
  float: right;
  margin-top: -50px;
}
</style>
