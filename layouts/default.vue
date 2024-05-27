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
              <li :class="{ active: currentPage === 'share' }">
                <NuxtLink to="/share" @click="navigateToPage('share')"
                  >Được Chia Sẻ</NuxtLink
                >
              </li>
            </ul>
          </div>
          <div class="show-main col-8">
            <!-- Hiển thị file -->
            <div class="card">
              <div class="top">
                <p>Drag & drop images or folders</p>
              </div>
              <div
                class="drag-area"
                @dragover.prevent="onDragOver"
                @dragleave.prevent="onDragLeave"
                @drop.prevent="onDrop"
              >
                <span v-if="!isDragging">
                  Drag & drop image here or
                  <span class="select" role="button" @click="selectFiles"
                    >Choose</span
                  >
                </span>
                <div v-else class="select">Drop images or folders here</div>
                <input
                  name="file"
                  type="file"
                  class="file"
                  ref="fileInput"
                  multiple
                  @change="onFileSelect"
                  webkitdirectory
                />
              </div>
              <div class="container">
                <div
                  v-for="(path, index) in folderPaths"
                  :key="index"
                  class="file-path"
                >
                  <span class="delete" @click="deleteImage(index)"
                    >&times;</span
                  >
                  <p>{{ path }}</p>
                </div>
              </div>
              <button type="button">Upload</button>
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
export default defineComponent({
  data() {
    return {
      images: [],
      isDragging: false,
      currentPage: "home",
      folderPaths: [],
    };
  },
  methods: {
    async onFileSelect(event) {
      const files = event.target.files;
      this.processFiles(files);
    },
    async processFiles(files) {
      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        this.folderPaths.push(file.webkitRelativePath || file.name);
      }
    },
    deleteImage(index) {
      this.folderPaths.splice(index, 1);
    },
    onDragOver(event) {
      this.isDragging = true;
      event.dataTransfer.dropEffect = "copy";
    },
    onDragLeave(event) {
      this.isDragging = false;
    },
    async onDrop(event) {
      event.preventDefault();
      this.isDragging = false;
      const items = event.dataTransfer.items;
      for (let i = 0; i < items.length; i++) {
        const item = items[i].webkitGetAsEntry();
        if (item) {
          await this.traverseFileTree(item, "");
        }
      }
    },
    async traverseFileTree(item, path) {
      if (item.isFile) {
        item.file((file) => {
          this.processFiles([file]);
        });
      } else if (item.isDirectory) {
        const dirReader = item.createReader();
        dirReader.readEntries(async (entries) => {
          for (let i = 0; i < entries.length; i++) {
            await this.traverseFileTree(entries[i], `${path}/${item.name}`);
          }
          if (!entries.length) {
            this.folderPaths.push(`${path}/${item.name}`);
          }
        });
      }
    },
    navigateToPage(page) {
      this.currentPage = page;
    },
  },
});
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
.card {
  position: relative;
  margin: 30px 0 35px 242px;
  width: 70%;
  min-height: 300px;
  max-height: 900px;
  padding: 10px;
  box-shadow: 0 0 5px #072638;
  border-radius: 5px;
  overflow: hidden;
}
.card .top {
  text-align: center;
  font-size: 25px;
}
.card p {
  font-weight: bold;
  color: #353b39;
}
.card button {
  outline: 0;
  border: 0;
  color: #fff;
  border-radius: 4px;
  font-weight: 400;
  padding: 8px 13px;
  width: 100%;
  background: #353b39;
}
.card .drag-area {
  height: 150px;
  border-radius: 5px;
  border: 2px dashed #353b39;
  background: #fff;
  color: #353b39;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: center;
  -webkit-user-select: none;
  margin-top: 10px;
  font-size: 25px;
}
.card .drag-area .visible {
  font-size: 18px;
}
.card .select:hover {
  opacity: 0.6;
}
.card .container {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;
  max-height: 200px;
  position: relative;
  margin-bottom: 8px;
  padding-top: 15px;
}
.card .file-path {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.card .file-path p {
  margin: 0;
  padding: 0;
  margin-left: 10px;
}
.card .file-path .delete {
  cursor: pointer;
  font-size: 20px;
  color: #353b39;
}
.card input,
.card .drag-area .on-drop,
.card .drag-ares.dragover .visible {
  display: none;
}
.delete {
  z-index: 999;
  color: #353b39;
}
.pad {
  padding-right: 10%;
}
</style>
