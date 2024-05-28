<script setup>
definePageMeta({
  layout: "",
});
</script>
<template>
  <div>
    <header class="top-menu shadow-sm bg-white">
      <div class="row flex items-center">
        <nav class="container flex items-center">
          <div class="row logo pad">
            <img
              src="/image/logo.png"
              alt="logo"
              class="logo-detail col-lg-1"
            />
            <NuxtLink to="/" class="header-name col-lg-6">SuperCloud</NuxtLink>
          </div>
          <div class="searchs pad">
            <SearchPackage />
          </div>
          <div class="login-register pad">
            <div class="section-user">
              <ul>
                <li>
                  <i class="fa-regular fa-circle-user"></i>
                  <NuxtLink to="/logins"> Log Out </NuxtLink>
                </li>
              </ul>
            </div>
          </div>
        </nav>
      </div>
    </header>
    <main>
      <div class="color-main">
        <div class="row">
          <div class="nav-bar col-6">
            <ul class="navbar-vertical">
              <li :class="{ active: currentPage === 'home' }">
                <NuxtLink to="/" @click="navigateToPage('home')"
                  >Trang Chủ</NuxtLink
                >
              </li>
              <li :class="{ active: currentPage === 'about' }">
                <NuxtLink to="/about" @click="navigateToPage('about')"
                  >Gần Đây</NuxtLink
                >
              </li>
              <li :class="{ active: currentPage === 'mydrive' }">
                <NuxtLink to="/mydrive" @click="navigateToPage('mydrive')"
                  >Của tôi</NuxtLink
                >
              </li>
              <li :class="{ active: currentPage === 'shares' }">
                <NuxtLink to="/shares" @click="navigateToPage('shares')"
                  >Được Chia Sẻ</NuxtLink
                >
              </li>
            </ul>
          </div>
          <div class="show-main col-8">
            <!-- Hiển thị file -->
            <div class="cards">
              <div class="title-form">
                <p>Tên</p>
                <p>Ngày</p>
                <p>Size</p>
                <p></p>
              </div>
              <div
                v-for="(file, index) in uploadedFiles"
                :key="index"
                class="file-item"
              >
                <p>{{ file.name }}</p>
                <p>{{ file.date }}</p>
                <p>{{ file.size }}</p>

                <button @click="toggleDropdown(index)">:</button>
                <!-- Dropdown -->
                <div v-if="file.showDropdown" class="dropdown">
                  <ul>
                    <li><button @click="editFile(index)">Chia Sẻ</button></li>
                    <hr />
                    <li><button @click="deleteFile(index)">Xóa</button></li>
                    <hr />
                    <li><button @click="renameFile(index)">Đổi tên</button></li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <footer>
      <div class="row">
        <div class="col-lg-8 col-12">
          <ul class="list-menu-footer">
            <li class="menu-item-footer">
              <nuxt-link to="/" title="Giới thiệu SuperCloudFake"
                >@2024 Super Cloud</nuxt-link
              >
            </li>
            <li class="menu-item-footer">
              <nuxt-link to="/more-info/privacy-policy" target="_self"
                >Privacy Policy</nuxt-link
              >
            </li>
          </ul>
        </div>
      </div>
    </footer>
  </div>
</template>
<script>
import "@fortawesome/fontawesome-free/css/all.css";
import axios from "axios";

export default {
  data() {
    return {
      currentPage: "mydrive",
      uploadedFiles: [
        { name: "minh1", date: "10/3/2024", size: "20kb" },
        { name: "minh2", date: "1/3/2024", size: "1kb" },
      ], // Thêm mảng để lưu trữ thông tin về các file đã tải lên
    };
  },
  async mounted() {
    // Gửi yêu cầu GET để lấy danh sách các file đã tải lên
    try {
      const response = await axios.get("/api/uploaded-files");
      this.uploadedFiles = response.data.map((file) => ({
        name: file.name,
        date: file.date,
        size: file.size,
      }));
    } catch (error) {
      console.error("Failed to fetch uploaded files:", error);
    }
  },
  methods: {
    navigateToPage(page) {
      this.currentPage = page;
    },
    toggleDropdown(index) {
      // Đảo ngược trạng thái hiển thị dropdown cho file tương ứng
      this.uploadedFiles[index].showDropdown =
        !this.uploadedFiles[index].showDropdown;
    },
    editFile(index) {
      // Xử lý tùy chọn "Chia Sẻ"
      console.log("Chia Sẻ", this.uploadedFiles[index]);
    },
    deleteFile(index) {
      // Xử lý tùy chọn "Xóa"
      console.log("Xóa file", this.uploadedFiles[index]);
    },
    renameFile(index) {
      // Xử lý tùy chọn "Đổi tên"
      console.log("Đổi tên file", this.uploadedFiles[index]);
    },
  },
};
</script>
<style>
.header-name {
  color: #353b39;
  font-size: 50px;
  font-family: Labernia Condensed Light;
}
.row {
  display: flex;
  padding: 10px;
}
.top-menu {
  background-color: #e1e1e1;
}
.logo-detail {
  height: 100px;
  padding-right: 10px;
}
.nav-bar {
  width: 20%;
  position: relative;
  top: 45px;
}
.color-main {
  background-color: #e1e1e1;
  z-index: -1;
}
.show-main {
  background-color: #fff;
  border-radius: 50px;
  width: 100%;
  height: 100%;
  z-index: 1;
}
.navbar-vertical {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
.navbar-vertical li {
  display: flex;
  justify-content: center;
  padding-bottom: 15px;
}
.navbar-vertical li a {
  display: block;
  padding: 10px;
  text-decoration: none;
  color: #353b39;
  font-size: 25px;
}
.navbar-vertical li a:hover {
  width: 80%;
  display: flex;
  justify-content: center;
  background-color: #353b39;
  color: #fff;
  border-radius: 20px;
}
.navbar-vertical li.active a {
  width: 80%;
  display: flex;
  justify-content: center;
  background-color: #353b39;
  color: #fff;
  border-radius: 20px;
}
.login-register {
  display: flex;
  align-items: center;
  position: relative;
  margin-right: -35%;
}
.section-user ul {
  display: flex;
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-size: 24px;
  font-weight: 500;
}
.section-user ul li {
  margin-right: 10px;
}
.section-user ul li:last-child {
  margin-right: 0;
}
.list-menu-footer {
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-size: 20px;
}
.menu-item-footer {
  display: inline-block;
  margin-right: 20px;
}
.menu-item-footer:last-child {
  margin-right: 0;
}
.cards {
  min-height: 500px;
}
.pad {
  padding-right: 10%;
}
/* từng khối hiển thị ở đây */
.file-item {
  position: relative;
  width: 90%;
  top: 85px;
  left: 55px;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #dedede;
  display: flex;
  justify-content: space-between;
}
.file-item p {
  font-size: 16px;
  font-weight: bold;
  margin: 0;
}
.file-item:hover {
  background-color: #eaeaea;
}
.dropdown {
  position: absolute;
  z-index: 3;
  left: 84%;
  background-color: #fff;
  width: 14%;
  height: 81px;
  border: 2px #d4d4d4 solid;
  border-radius: 5px;
  text-align: center;
}
.title-form {
  display: flex;
  justify-content: space-between;
  position: relative;
  left: 70px;
  top: 70px;
  width: 90%;
}
</style>
