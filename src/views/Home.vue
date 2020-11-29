<template>
  <div class="home bg-cover">
    <div>
      <button
        style="margin-bottom:20px"
        class="pointer crud-btn nav-btn margin-20  nav-btn shadow"
        @click="viewAllData"
      >
        See list
      </button>
    </div>
    <div
      class="text-editor shadow"
      id="dragme"
      :style="{ top: dragTop, left: dragLeft }"
    >
      <div
        style="min-height:50px; background:#f2f2f2;display:flex; cursor:move; text-align"
        @mousedown.stop="onMouseClick($event)"
        @mouseup.stop="onMouseUp($event)"
        @mousemove.stop="onMouseMove($event)"
      >
        <p>Hold and drag here to move editor</p>
      </div>
      <div
        contenteditable="true"
        style="text-align:left; min-height:300px; padding:10px"
        class="edit-box"
        @click.stop="removeImageBorder"
      >
        {{ localStorageContent }}
        <div>
          <img
            :src="imageFile"
            alt=""
            :height="imageHeight"
            :class="{ 'border-active': imageBorder }"
            v-if="this.imageFile !== null"
            @click.stop="addBorder"
          />
        </div>
      </div>

      <div
        style="margin:10px; min-height:50px; text-align:right; overflow:hidden"
      >
        <p class="animate__animated animate__bounceInUp" v-if="dataSaved">
          data saved
        </p>
      </div>

      <div class="text-menu shadow">
        <button class="pointer save-btn" @click="saveContent($event)">
          save
        </button>
        <div>
          <input
            type="color"
            v-model="textColor"
            class="pointer"
            title="change text color"
            @change="getTextColor"
          />
        </div>
        <div>
          <label for="imageInput" class="pointer" title="upload image"
            ><i class="fa fa-file-image-o fa-icon"></i
          ></label>
          <input
            type="file"
            accept=".pdf,.jpg,.png,.jpeg"
            @change="insertImage($event)"
            id="imageInput"
            style="opacity:0;width: 0px;"
          />
        </div>
        <div v-if="imageFile !== null">
          <select
            @change.stop="changeFontSize($event)"
            v-model="fontSize"
            class="pointer"
            style="min-height:30px"
            title="change image width"
          >
            <option v-for="(size, index) in imageSizes" :key="index+1"  :selected="index == 0">
              {{ size }}</option
            >
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",

  data() {
    return {
      imageFile: null,
      textColor: "",
      imageSizes: [100, 120, 193, 219],
      fontSize: 18,
      dragTop: "100px",
      dragLeft: "200px",
      prevX: 20,
      prevY: 20,
      dataSaved: false,
      mouseDown: false,
      imageBorder: false,
      localStorageContent: null,
      imageHeight: 100,
      editStatus: false,
      editData: {},
    };
  },
  components: {
    HelloWorld,
  },

  methods: {
    onMouseClick(e) {
      this.prevX = e.clientX;
      this.prevY = e.clientY;
      this.mouseDown = true;
    },
    onMouseUp(e) {
      this.mouseDown = false;
    },
    onMouseMove(e) {
      if (this.mouseDown) {
        this.dragLeft =
          Number(this.dragLeft.substring(0, this.dragLeft.length - 2)) +
          (e.clientX - this.prevX) +
          "px";
        this.dragTop =
          Number(this.dragTop.substring(0, this.dragTop.length - 2)) +
          (e.clientY - this.prevY) +
          "px";
      }
      this.prevX = e.clientX;
      this.prevY = e.clientY;
    },

    removeImageBorder() {
      this.imageBorder = false;
    },
    addBorder() {
      this.imageBorder = true;
    },
    viewAllData(e) {
      this.$router.push("/list");
    },
    getTextColor(e) {
      document.execCommand("styleWithCSS", false, true);
      document.execCommand("foreColor", false, this.textColor);
    },
    changeFontSize(e) {
      this.imageHeight = this.fontSize;
      document.execCommand("fontSize", false, this.fontSize);
    },

    saveContent(e) {
      this.dataSaved = true;
      console.log(
        e.target.parentNode.parentNode.querySelector(".edit-box").innerText
      );

      let savedObject = {
        text: e.target.parentNode.parentNode.querySelector(".edit-box")
          .innerText,
        image: this.imageFile !== null ? this.imageFile : null,
        imageHeight: this.imageFile !== null ? this.imageHeight : null,
      };

      let allSavedData = localStorage.getItem("allSavedDataFromLocalStorage");

      allSavedData = JSON.parse(allSavedData);


      if (this.editStatus) {
        let newIndex = allSavedData.indexOf(allSavedData[this.editData.index]);
        if (newIndex !== -1) {
          allSavedData[newIndex] = savedObject;
        }

        localStorage.setItem(
          "allSavedDataFromLocalStorage",
          JSON.stringify(allSavedData)
        );
        localStorage.setItem("editDataStatus", JSON.stringify(false));
        setTimeout(() => {
          this.$router.push("/list");
        }, 2000);
        return;
      }

      let localStorageArray =
        allSavedData == null ? [savedObject] : [savedObject, ...allSavedData];

      localStorage.setItem(
        "allSavedDataFromLocalStorage",
        JSON.stringify(localStorageArray)
      );
      setTimeout(() => {
        this.dataSaved = false;
      }, 2000);
      localStorage.setItem("editDataStatus", JSON.stringify(true));
      this.$router.push("/list");
    },


    insertImage(e) {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        this.imageFile = e.target.result;
      };
      reader.readAsDataURL(file);
    },
  },

  mounted() {
    let savedDataFromLocalStorage = localStorage.getItem("viewData");

    savedDataFromLocalStorage = JSON.parse(savedDataFromLocalStorage);
    let editStatusFromLocal = localStorage.getItem("editDataStatus");

    this.editStatus = JSON.parse(editStatusFromLocal);

    if (this.editStatus) {
      this.localStorageContent = savedDataFromLocalStorage.text;
      this.imageHeight = savedDataFromLocalStorage.imageHeight;
      this.editData = savedDataFromLocalStorage;
      if (savedDataFromLocalStorage.image !== null) {
        this.imageFile = savedDataFromLocalStorage.image;
      }
    }

  },
};
</script>

<style lang="scss" scoped>
$white: #fff;

.fa-icon {
  font-size: 35px;
}

.pointer {
  cursor: pointer;
}

.text-editor {
  text-align: left;
  background: $white;
  min-height: 400px;
  max-width: 500px;
  min-width: 500px;
  margin: 0 auto;
  position: absolute;
  box-shadow: 0 4px 8px rgba(59, 64, 69, 0.2) !important;
}

.save-btn {
  background: #185abc;
  border: 1px solid #185abc;
  color: #fff;
  height: 40px;
  min-width: 100px;
  border-radius: 5px;
  margin-right: 10px;
}

.text-menu {
  background: $white;
  min-height: 70px;
  justify-content: space-between;
  min-width: 100px;
  padding: 10px;
  display: flex;
  align-items: center;
  margin: 10px;
}

.border-active {
  border: 3px solid blue;
}
[contenteditable] {
  outline: 0px solid transparent;
}
</style>
