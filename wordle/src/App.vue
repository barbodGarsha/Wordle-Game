<script setup>
import { ref } from 'vue'

import TheHeader from './components/TheHeader.vue'
import Column from './components/Column.vue'
import Row from './components/Row.vue'
import Keyboard from './components/Keyboard.vue'


//WORDS:
const words = [
    "Latch", "Braid", "Snack", "Amber", "Swirl",
    "Plumb", "Crisp", "Gears", "Blend", "Quirk",
    "Frost", "Crown", "Pilot", "Roast", "Trail",
    "Flame", "Charm", "Shade", "Quest", "Zebra",
    "Fable", "Globe", "Crane", "Lunar", "Swift",
    "Lemon", "Vivid", "Oasis", "Bench", "Dance",
    "Cloud", "Rider", "Knots", "Feast", "Pouch",
    "Grace", "Crush", "Sweep", "Joust", "Gleam",
    "Chill", "Blitz", "Lunar", "Civic", "Spark",
    "Fable", "Cameo", "Blend", "Prism", "Twist",
    "Wings", "Brave", "Brisk", "Jewel", "Pulse",
    "Ample", "Heart", "Mirth", "Diver", "Flint",
    "Coral", "Funky", "Glide", "Jumbo", "Merry",
    "Peach", "Forge", "Wedge", "Mirth", "Glint",
    "Plush", "Quest", "Happy", "Wings", "Flare",
    "Whale", "Lunar", "Oasis", "Globe", "Cloud",
    "Frost", "Woven", "Chill", "Storm", "Daisy",
    "Crown", "Blaze", "Vivid", "Brave", "Bloss",
    "Frost", "Lunar", "Dream", "Mirth", "Bloom",
    "Tiger", "Spark", "Twist", "Shade", "Jewel",
    "Charm", "Roast", "Flame", "Swift", "Bench",
    "Grain", "Sword", "Plaid", "Spine", "Stove",
    "Beard", "Crate", "Spoke", "Wrist", "Crack",
    "Table", "Horse", "Flock", "Spare", "Knots",
    "Plume", "Maple", "Chase", "Slate", "Drift",
    "Chest", "Mouth", "Break", "Queen", "Fleet",
    "Vocal", "Porch", "Blind", "Bloom", "Rider",
    "Pulse", "Quiet", "Shirt", "Fresh", "Guide",
    "Music", "Taste", "Click", "Field", "Wheel"
  ]

//GLOBALS ================================================================================
const maxColumnNum = ref(5)
const maxRowNum = ref(5)
const rowIndex = ref(0)
const columnIndex = ref(0)
const currentRow = ref([])

//NOTE: these are for now just for testing and the structure of this code will change
const isCorrect = ref([])
const isWrong = ref([])
const isNotRightPos = ref([])
for(let i = 0; i < maxColumnNum.value; i++) {
  isCorrect.value[i] = false
  isWrong.value[i] = false
  isNotRightPos.value[i] = false
}

//FUNCTIONS =============================================================================

/**
 * Returns a random integer between min (inclusive) and max (inclusive).
 * The value is no lower than min (or the next integer greater than min
 * if min isn't an integer) and no greater than max (or the next integer
 * lower than max if max isn't an integer).
 * Using Math.round() will give you a non-uniform distribution!
 */
 function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

function newGameInit() {
  const newWorld = words[getRandomInt(0, words.length - 1)]
  console.log(newWorld)
}

newGameInit()
//EVENTS ================================================================================
document.addEventListener('keyup', (e) => {
  
  const val = e.key.toUpperCase()
  if(val === "ENTER") {
    //TODO: check the row and react based on the state of the game
    if(columnIndex.value === maxColumnNum.value) {
      isCorrect.value[0] = true
      isNotRightPos.value[1] = true
      isWrong.value[2] = true
    }
    else {
    }
  }
  else if(val === "BACKSPACE") {
    columnIndex.value--
    currentRow.value[columnIndex.value] = ''
  }
  else if(columnIndex.value === maxColumnNum.value) { return }
  else if(val.length === 1 && (/[A-Z]/).test(val)) {
    currentRow.value[columnIndex.value] = val
    columnIndex.value++
  }
})


</script>

<template>
  <TheHeader></TheHeader>

  <main class="main">
      
    <Row>
      <Column :value="currentRow[0]" :isCorrect="isCorrect[0]" :isNotRightPos="isNotRightPos[0]" :isWrong="isWrong[0]"></Column>
      <Column :value="currentRow[1]" :isCorrect="isCorrect[1]" :isNotRightPos="isNotRightPos[1]" :isWrong="isWrong[1]"></Column>
      <Column :value="currentRow[2]" :isCorrect="isCorrect[2]" :isNotRightPos="isNotRightPos[2]" :isWrong="isWrong[2]"></Column>
      <Column :value="currentRow[3]" :isCorrect="isCorrect[3]" :isNotRightPos="isNotRightPos[3]" :isWrong="isWrong[3]"></Column>
      <Column :value="currentRow[4]" :isCorrect="isCorrect[4]" :isNotRightPos="isNotRightPos[4]" :isWrong="isWrong[4]"></Column>
    </Row>
    
    <Row>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
    </Row>
    
    <Row>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
    </Row>
    
    <Row>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
    </Row>
    
    <Row>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
      <Column value="X"></Column>
    </Row>

    <Keyboard></Keyboard>

</main>
</template>

<style lang="scss">
  
  //RESETS
  html {
      box-sizing: border-box;
  }
    
  *, *:before, *:after {
      box-sizing: inherit;
  }

  body {
      background-color: white;
      margin: 0;
      padding: 0;
      font-family: sans-serif;
  }
  a, li, h1, h2, p {
      text-decoration: none;
      margin: 0;
      padding: 0;
      z-index: 9999;
  }

  ul {
      display: inline;  
      margin: 0;
      padding: 0;
  }
  li {
      display: inline;
      list-style-type: none;
      margin: 0;
      padding: 0; 
  }


  .main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    padding: 2rem;
  }
</style>
