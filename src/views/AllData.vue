<template>
  <div class="home">
    <div style="max-width:700px; margin:0 auto">
      <button class="pointer crud-btn nav-btn margin-20 nav-btn shadow" @click="$router.go(-1)"  v-if="savedData.length > 0"><i class="fa fa-arrow-circle-left"></i> Go back</button>

      <div class="animate__animated animate__fadeIn margin-20 success-info-div" v-if="editDataIsSuccessfull">

        <p class=""> <i class="fa fa-info-circle"></i> Data updated</p>
        <i class="fa fa-times-circle close-btn pointer" @click="editDataIsSuccessfull = false"></i>
      </div>
      <div  class="no-data-div" v-if="savedData.length < 1">
        <p>Sorry you don't have any saved data</p>
        <button @click="createData()" class="crud-btn margin-20 pointer nav-btn shadow" >
          Create Data
        </button>
      </div>

    <div name="custom-classes-transition" enter-active-class="animate__animated animate__backInDown" leave-active-class="animate__animated  animate__backOutDown" :duration="{ enter: 1000, leave: 1000 }" is="transition-group" v-else>
      <div
      class="data-item shadow"
     
        v-for="(data, index) in savedData"
        :key="index+1"
      >
        <p>{{ data.text }}</p>
        <p style="font-size:12px; color:#bab2b2"> {{ data.dateCreated | formatTime}}</p>
        <img :src="data.image" :height="data.imageHeight" alt="" v-if="data.image !== null" />
        <div class="margin-20" style="display:flex">
            <button @click="viewData(data, index)" class="crud-btn view-bg">view</button>
          <button @click="editData(data, index)" class="crud-btn edit-bg">
            edit
          </button>
        

          <button @click="deleteData(index)" class="crud-btn delete-bg">delete</button>
        </div>
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
      savedData: [],
      editDataIsSuccessfull:false
    };
  },

  methods: {
    deleteData(index) {
      this.savedData.splice(index, 1);
      localStorage.setItem(
        "allSavedDataFromLocalStorage",
        JSON.stringify(this.savedData)
      );
    },
    editData(data, index){
        data.index = index;

      localStorage.setItem("viewData", JSON.stringify(data));

localStorage.setItem('editDataStatus', JSON.stringify(true))
    localStorage.setItem("editDataDiv", JSON.stringify(false));
this.$router.push('/')


    },
    viewData(data, index) {
      data.index = index;
      localStorage.setItem("viewData", JSON.stringify(data));
      this.$router.push("/view/" + index);
    },
    createData() {
      this.$router.push("/");
    },

   
  },
  filters: {
    formatTime: function(date) {
           return moment(date).format("Do MMM YYYY, h:mma");
    },
  },
  mounted() {
    let savedData = localStorage.getItem("allSavedDataFromLocalStorage");
    savedData = JSON.parse(savedData);
    let editStatus = localStorage.getItem("editDataSuccessDiv");
    this.editDataIsSuccessfull = JSON.parse(editStatus);

    this.savedData = savedData;
  },

  deactivated() {
    this.editDataIsSuccessfull = false
  },
};
</script>

<style lang="scss" scoped>
.no-data-div{
text-align:center; min-height:500px; display:flex; justify-content:center; align-items:center; flex-flow:column

}

.success-info-div{
  background:#cbe2f9; min-height:60px; display:flex; justify-content:center;    color: #0b5cad; border-radius:5px;
  position: relative;
}

.close-btn{

      position: absolute;
    right: 2%;
    font-size: 24px;
    top: 26%;
}
</style>
