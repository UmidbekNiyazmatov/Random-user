<script setup>
import { ref , computed  } from "vue";

const firstname = ref("Vickie");
const lastname = ref("Stone");
const email  = ref("vikie.stone@gmail.com");
const gender = ref("female ") ;
const phoneNomber = ref("(434) 771-4797");
const img = ref("https://randomuser.me/api/portraits/men/75.jpg");

const fullName = computed(() => `${firstname.value} ${lastname.value} `);

const checkGender = computed(() =>({
  "border-blue-500": gender.value === "male",
  "border-pink-500": gender.value === "female"
}))

const randomUser = async () => {
  try {
    const res = await fetch("https://randomuser.me/api/");
    if (!res.ok){
      throw new  Error(`Server error: ${res.status}`)
    }
    
    const { results } = await res.json();

    const user = results[0];


    firstname.value = user.name.first;
    lastname.value = user.name.last;
    email.value = user.email;
    gender.value = user.gender;
    phoneNomber.value = user.phone;
    img.value = user.picture.large;
  
  } catch (error) {
    console.log("error", error);
  }
}
</script>

<template>
<div class="flex justify-center items-center min-h-screen" >
  <div class="flex flex-col justify-center items-center" >
    <div class="relative mb-8" >
      <div class="absolute left-1/3 bg-white rounded-full px-4  text-lg transform -translater-x-1/2 " > 
        {{gender}}
      </div>
      <img 
        class="w-64 h-64 rounded-full border-8  "
        :class="checkGender"
       :src="img" :alt="firstname">

    </div>
    <div class="text-center space-x-1" >
      <h1 class="text-4xl font-bold" > Hi, My name is {{fullName}} </h1>
      <p class="text-lg text-gray-500" >{{email}}</p>
      <p class="text-lg text-gray-500" >{{phoneNomber}}</p>
    </div>
    <button @click="randomUser"
     class="bg-black text-white px-6 py-2 rounded font-bold mt-6" >Random user</button>
  </div>
</div>
</template>

<style scoped>

</style>
