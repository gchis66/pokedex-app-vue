<template>
<div class="w-full flex justify-center">
  <input type="text" placeholder="Enter Pokemon here"
  class="mt-10 p-2 border-blue-500 border-2"
  v-model="text"/>
</div>
<div class="mt-10 p-4 flex flex-wrap justify-center">
<div class="ml-4 text-2x text-blue-500"
v-for="(pokeman, idx) in filteredPokemon"
:key="idx"
>
<router-link :to="`/about/${urlIdLookup[pokeman.name]}`">
{{pokeman.name}}
</router-link>
</div>

</div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from 'vue';
export default {
  name: 'Home',
  setup(){

    const state = reactive({
      pokemon: [],
      urlIdLookup:{},
      text:"",
      filteredPokemon: computed(() => updatePokemon())
    })

    function updatePokemon(){
      if(!state.text){
        return []
      }
      return state.pokemon.filter((pokeman) =>
        pokeman.name.includes(state.text)
      )
    }

    fetch("https://pokeapi.co/api/v2/pokemon?limit=500")
    .then((res) => res.json())
    .then((data) => {
      console.log(data);
      state.pokemon = data.results;
      state.urlIdLookup = data.results.reduce((acc, cur, idx) => 
        acc = {...acc, [cur.name]:idx+1}

        ,{})
    })
    return {...toRefs(state)}
  }
}
</script>
