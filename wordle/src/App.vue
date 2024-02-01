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
const rowValues = ref([])

var currentWord = []

//NOTE: these are for now just for testing and the structure of this code will change
const isCorrect = ref([])
const isWrong = ref([])
const isNotRightPos = ref([])
rowValues.value = createTwoDimArr(maxRowNum.value, maxColumnNum, '')

isCorrect.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isWrong.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isNotRightPos.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)


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

function createTwoDimArr(rowNum, ColumnNum, initValue) {
  let arr = []
  for(let i = 0; i < rowNum; i++) {
    arr[i] = []
    for(let j = 0; j < ColumnNum; j++) {
      arr[i][j] = initValue
    } 
  }
  return arr
}

function newGameInit() {
  const newWord = words[getRandomInt(0, words.length - 1)].toUpperCase()
  const newWordArr = [...newWord]

  for(let i = 0; i < 5; i++) {
    currentWord[i] = newWordArr[i]
  }
  
  rowValues.value = createTwoDimArr(maxRowNum.value, maxColumnNum, '')

  isCorrect.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
  isWrong.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
  isNotRightPos.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
  
  columnIndex.value = 0
  rowIndex.value = 0
  
  console.log(currentWord)
}

newGameInit()
//EVENTS ================================================================================
document.addEventListener('keydown', (e) => {
  
  const val = e.key.toUpperCase()
  if(val === "ENTER") {
    //TODO: check the row and react based on the state of the game
    if(columnIndex.value === maxColumnNum.value) {
      let columsState = [0, 0, 0, 0, 0]
      let rightAnswers = 0

      for(let i = 0; i < 5; i++) {
        
        if(currentWord.includes(rowValues.value[rowIndex.value][i])) { columsState[i] = 1}
        if(rowValues.value[rowIndex.value][i] === currentWord[i]) { columsState[i] = 2}
        
        if(columsState[i] === 0) {  
          isWrong.value[rowIndex.value][i] = true
        }
        else if(columsState[i] === 1) {
          isNotRightPos.value[rowIndex.value][i] = true
        }
        else if(columsState[i] === 2) {
          isCorrect.value[rowIndex.value][i] = true
          rightAnswers++
        }
      }
      rowIndex.value++
      columnIndex.value = 0
      if(rightAnswers === 5) {
        alert("WON")
      }
      else if(rowIndex.value === maxRowNum.value) {
        alert("LOST")
      }
    }
    else {
    }
  }
  else if(val === "BACKSPACE") {
    columnIndex.value--
    rowValues.value[rowIndex.value][columnIndex.value] = ''
  }
  else if(val === "ESCAPE") {
    newGameInit()
  }
  else if(columnIndex.value === maxColumnNum.value) { return }
  else if(val.length === 1 && (/[A-Z]/).test(val)) {
    rowValues.value[rowIndex.value][columnIndex.value] = val
    columnIndex.value++
  }
})


</script>

<template>
  <TheHeader></TheHeader>

  <main class="main">
     
    <Row v-for="i in maxRowNum">
      <Column v-for="j in maxColumnNum" :value="rowValues[i - 1][j - 1]" :isCorrect="isCorrect[i - 1][j - 1]" :isNotRightPos="isNotRightPos[i - 1][j - 1]" :isWrong="isWrong[i - 1][j - 1]"></Column>
    </Row>
    

</main>
</template>

<style lang="scss">
  
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;700&display=swap');

  //RESETS
  html {
      box-sizing: border-box;
      
      font-family: 'Poppins', sans-serif;
  }
    
  *, *:before, *:after {
      box-sizing: inherit;
  }

  body {
      background-color: white;
      margin: 0;
      padding: 0;
      font-family: 'Poppins', sans-serif;
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
