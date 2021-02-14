<template>
  <div class="about">
      <div class="p-4">
    <router-link class="flex justify-center text-2xl text-yellow-700" to="/">Back</router-link>
  </div>
    <div v-if="pokeman" class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center">
      <h3 class="text-2xl text-green-900 uppercase">{{pokeman.name}}</h3>
      <div class="flex justify-center">
        <img class="w-58" :src="pokeman.sprites.front_default" alt="front image of pokemon">
        <img class="w-58" :src="pokeman.sprites.back_default" alt="back image of pokemon">
      </div>
      <h3 class="text-yellow-400">Types</h3>
      <div v-for="(type,idx) in pokeman.types" :key="idx">
        <h5 class="text-blue-900">{{type.type.name}}</h5>
      </div>
    </div>
  </div>
</template>


<script>
import {useRoute} from 'vue-router';
import {reactive, toRefs} from 'vue';

export default {
  setup() {
  const route= useRoute();
  const state = reactive({
    pokeman: null
  })

  fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
  .then((res) => res.json())
  .then((data) => {
    console.log(data);
    state.pokeman = data;
  })

  return {...toRefs(state)}

  }
}


</script>