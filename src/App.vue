<template>
  <div id="app">
    <div id="nav">
   
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
$white : #fff;
$themeBlue:#185abc;
$themeRed:#E15171;
$black:#000;
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
 
}

.shadow{
  box-shadow: 0 2px 8px rgba(59, 64, 69, 0.2) !important;
}
.crud-btn {
  border-radius:10px;
  height: 30px;
  padding: 5px;
  color:$white;
  border:1px solid $white ;
  min-width: 100px;
  cursor: pointer;
  margin-right: 10px;
}

.view-bg{
  background: $themeBlue;
}

.delete-bg{
  background: $themeRed;
}

.edit-bg{
  background:#96cecf;
}

.margin-20{
  margin-top:20px;
}
.nav-btn{
  background:$white;
  color:$black;
}
.data-item{
  text-align:left; min-height:100px; background:#fff; margin-top:40px; padding:10px; margin-bottom:12px; overflow:hidden; border-radius:12px
}
</style>
