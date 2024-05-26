<template>
  <div class="Wrap">
    <div class="left">
      <div class="logof">
        <img src="/image/logo.png" />
      </div>

      <div class="text">
        <div class="forgetTitle" align="center">Quên Mật Khẩu</div>
      </div>
    </div>
    <div class="right">
      <div class="sub-content">
        <div class="supercloud" align="center">SuperCloud</div>
        <div class="textfield" name="email">
          <input
            class="EmailText"
            type="text"
            name="txtEmail"
            placeholder="Email đăng kí tài khoản"
          />
        </div>

        <div class="button">
          <button class="NextButton" type="submit" name="butNext">Tiếp</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "logins",
});
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
  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.1);
}
.logo {
  margin-left: auto;
  background-color: white;
  border-radius: 30px;
}

.forgetTitle {
  font-size: 62px;
  font-family: "Inria Serif", serif;
  padding-bottom: 20px;
  margin-right: 20px;
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
  margin-top: 100px;
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
  margin-top: 85px;
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
