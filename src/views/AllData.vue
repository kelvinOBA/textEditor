<template>
  <div class="home">
    <div style="max-width:700px; margin:0 auto">
      <button class="pointer crud-btn nav-btn margin-20 nav-btn shadow" @click="$router.go(-1)"  v-if="savedData.length > 0"> Go back</button>
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
        <img :src="data.image" :height="data.imageHeight" alt="" v-if="data.image !== null" />
        <div class="margin-20">
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
export default {
  name: "Home",

  data() {
    return {
      savedData: [],
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

  mounted() {
    let savedData = localStorage.getItem("allSavedDataFromLocalStorage");
    savedData = JSON.parse(savedData);

    this.savedData = savedData;
  },
};
</script>

<style lang="scss" scoped>
.no-data-div{
text-align:center; min-height:500px; display:flex; justify-content:center; align-items:center; flex-flow:column

}

</style>
