<template>
    <div class="hero">
        <h1>I am home page.</h1>
        <h3 class="create_btn btn btn-primary"><NuxtLink to="/students"  class="createStudent">STUDENT PAGE</NuxtLink></h3>
    </div>

</template>


<script setup lang="ts">
useHead({
  title: 'Home Page'
})

</script>

<style>
body{
  background:url(/pages/img/solid.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  color:white;
}
.hero{
  height: 87vh;
  display:flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.createStudent{
  color:white;
  list-style: none;
  text-decoration: none;
}

.create_btn{
  padding:8px 15px;
  margin-top:10px;
  border:2px solid white;
  border-radius:20px;
}
</style>