<template>
  <div class="home">
    <div style="max-width:700px; margin:0 auto">
      <button class="pointer crud-btn nav-btn margin-20 shadow" @click="$router.go(-1)">Go back</button>
   
      <div
       
       class="shadow data-item"   
      >
        <p>{{ savedData.text }}</p>
        <img :src="savedData.image" :height="savedData.imageHeight" alt="" v-if="savedData.image !== null" />
        <div style="margin-top:10px">
           <button @click="editData()" class="crud-btn edit-bg">
            edit
          </button>
        

          <button @click="deleteData()" class="crud-btn delete-bg">delete</button>
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
      savedData: {},
      allSavedData:[]
    };
  },

  methods: {

    deleteData( ){


this.allSavedData.splice(this.savedData.index, 1);
localStorage.setItem('allSavedDataFromLocalStorage', JSON.stringify(
this.allSavedData))
this.$router.push('/list')
    },

    editData(data){
localStorage.setItem('editDataStatus', JSON.stringify(true))

this.$router.push('/')


    }
  },    

  mounted() {
    let AllSavedDataFromLocalStorage = localStorage.getItem("allSavedDataFromLocalStorage");
    let savedDataFromLocalStorage = localStorage.getItem("viewData");
    savedDataFromLocalStorage = JSON.parse(savedDataFromLocalStorage);
    AllSavedDataFromLocalStorage = JSON.parse(AllSavedDataFromLocalStorage);

    this.savedData = savedDataFromLocalStorage;
    this.allSavedData = AllSavedDataFromLocalStorage;

    console.log(this.savedData)
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
