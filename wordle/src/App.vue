<script setup>
import { ref } from 'vue'

import TheHeader from './components/TheHeader.vue'
import Column from './components/Column.vue'
import Row from './components/Row.vue'
import Keyboard from './components/Keyboard.vue'
import Button from './components/Button.vue'
import Overlay from './components/Overlay.vue'
import ToggleButton from './components/ToggleButton.vue'

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

// ---- BOARD ----
const maxColumnNum = ref(5)
const maxRowNum = ref(5)
const rowIndex = ref(0)
const columnIndex = ref(0)
const rowValues = ref([])

const isCorrect = ref([])
const isWrong = ref([])
const isNotRightPos = ref([])
rowValues.value = createTwoDimArr(maxRowNum.value, maxColumnNum, '')

isCorrect.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isWrong.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isNotRightPos.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)


// ---- GENERAL ----
const darkModeEnabled = ref(false)
const settingsHidden = ref(true)

// ---- GAME STATE ----
const gameWon = ref(false)
const gameLost = ref(false)

var currentWord = []

//FUNCTIONS =============================================================================

// ---- TOOLS ----
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

// ---- ACTIONS ----
function darkModeChange(state) {
  darkModeEnabled.value = state
}

function openSettings () {
  settingsHidden.value = false
}

function closeSettings() {
  settingsHidden.value = true
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
  
  gameLost.value = false
  gameWon.value = false

  console.log(currentWord)
}

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
        gameWon.value = true
      }
      else if(rowIndex.value === maxRowNum.value) {
        gameLost.value = true
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

//COOKIES ===============================================================================
function setCookie(name, value, ex) {
  const date = new Date();
  date.setTime(date.getTime() + (ex*24*60*60*1000));
  let expires = "expires="+ date.toUTCString();
  document.cookie = name + "=" + value + ";" + expires + ";path=/";
}

function getCookie(cookieName) {
  let name = cookieName + "=";
  let decodedCookie = decodeURIComponent(document.cookie);
  let ca = decodedCookie.split(';');
  for(let i = 0; i <ca.length; i++) {
    let c = ca[i];
    while (c.charAt(0) == ' ') {
      c = c.substring(1);
    }
    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length);
    }
  }
  return "";
}

function deleteCookie(cookieName) {
  document.cookie = cookieName + "=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
}


// MAIN =================================================================================
newGameInit()

</script>

<template>
  <TheHeader :dark-mode="darkModeEnabled"></TheHeader>

  <main :class="['main', { 'main--darkmode' : darkModeEnabled }]">
     
    <Overlay @background-clicked="closeSettings" :hidden="settingsHidden">
      <div class="settings">
        <div class="settings__section">
          <p class="settings__section__p">Dark Mode</p>  
          <ToggleButton @toggled="darkModeChange"></ToggleButton>
        </div>
      </div>
    </Overlay>

    <Overlay :hidden="!gameWon">
      <div class="you-won">
        <p class="you-won__message">You Won</p>
      </div>
    </Overlay>

    <Overlay :hidden="!gameLost">
      <div class="you-lost">
        <p class="you-lost__message">You Lost</p>
      </div>
    </Overlay>
    
    <div class="main__nav">
      <Button @clicked="openSettings" :isIconOnly="true" iconName="setting.png" :hasRotaionAnimation="true"></Button>
      <Button :isIconOnly="true" iconName="stats.png" :has-mirror-animation="true"></Button>
      <Button :isIconOnly="true" iconName="help.png" :has-mirror-animation="true"></Button>
    </div>

    <div class="main__section">
      <Row v-for="i in maxRowNum">
        <Column v-for="j in maxColumnNum" :value="rowValues[i - 1][j - 1]" :isCorrect="isCorrect[i - 1][j - 1]" :isNotRightPos="isNotRightPos[i - 1][j - 1]" :isWrong="isWrong[i - 1][j - 1]"></Column>
      </Row>
    </div>
    
    <div class="main__section">
      <Keyboard></Keyboard>
    </div>

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
      overflow: hidden;
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


  .settings {
    display: flex;
    justify-content: center;
    align-items: center;

    width: 100%;
    .settings__section {
      display: flex;
      justify-content: space-between;
      align-items: center;

      padding: 2rem;
      border-radius: 15px;
      border: solid 1px #1E4652;
      width: 90%;
      .settings__section__p {
        font-size: 1.2rem;
      }
    }
  }

  .you-won {
    display: flex;
    justify-content: center;
    align-items: center;

    .you-won__message {
      color: 1E4652;
      text-transform: uppercase;

      font-size: 3rem;
      font-weight: bold;
    }
  }

  .you-lost {
    display: flex;
    justify-content: center;
    align-items: center;

    .you-lost__message {
      color: 1E4652;
      text-transform: uppercase;

      font-size: 3rem;
      font-weight: bold;
    }
  }

  .main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    padding: 2rem;

    .main__section {
      margin: 1rem 0;
    }

    .main__nav {
      display: flex;
      justify-content: space-evenly;
      align-items: center;

      width: 15rem;
      padding: 1rem;
    }
  }
  .main--darkmode {
    background-color: #4A6A74;
  }
</style>
