<template>
  <div class="home">
    <div style="max-width:700px; margin:0 auto">
      <button
        class="pointer crud-btn nav-btn margin-20 shadow"
        @click="$router.go(-1)"
      >
        <i class="fa fa-arrow-circle-left"></i> Go back
      </button>

      <div class="shadow data-item animate__animated animate__fadeInUp" v-if="savedData">
        <p>{{ savedData.text }}</p>
          <p style="font-size:12px; color:#bab2b2"> {{ savedData.dateCreated | formatTime}}</p>
        <img
          :src="savedData.image"
          :height="savedData.imageHeight"
          alt=""
          v-if="savedData.image !== null"
        />
        <div style="margin-top:10px; display:flex">
          <button @click="editData()" class="crud-btn edit-bg">
            edit
          </button>

          <button @click="deleteData()" class="crud-btn delete-bg">
            delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "Home",

  data() {
    return {
      savedData: {},
      allSavedData: [],
    };
  },

  methods: {
    deleteData() {
      this.allSavedData.splice(this.savedData.index, 1);
      localStorage.setItem(
        "allSavedDataFromLocalStorage",
        JSON.stringify(this.allSavedData)
      );
      this.$router.push("/list");
localStorage.setItem('editDataStatus', JSON.stringify(false))
     localStorage.setItem("viewData", JSON.stringify(null));
    },

    editData(data) {
      localStorage.setItem("editDataStatus", JSON.stringify(true));

      this.$router.push("/");
    },
  },

    filters: {
    formatTime: function(date) {
       return moment(date).format("ddd, D-MM-YYYY, h:mma");
    },
  },

  mounted() {
    let AllSavedDataFromLocalStorage = localStorage.getItem(
      "allSavedDataFromLocalStorage"
    );
    let savedDataFromLocalStorage = localStorage.getItem("viewData");
    savedDataFromLocalStorage = JSON.parse(savedDataFromLocalStorage);
    AllSavedDataFromLocalStorage = JSON.parse(AllSavedDataFromLocalStorage);
  if(AllSavedDataFromLocalStorage.length < 1 ){
        this.$router.push('/list')
    }
    this.savedData = savedDataFromLocalStorage;
    this.allSavedData = AllSavedDataFromLocalStorage;

    localStorage.setItem("editDataSuccessDiv", JSON.stringify(false));


  
  },
};
</script>

<style lang="scss" scoped>
$blue: #f4f4;

.text-editor {
  text-align: left;
  background: $blue;
  min-height: 400px;
  max-width: 600px;
  margin: 0 auto;
}
</style>
