<script setup>
import { ref } from 'vue'

import TheHeader from '@/components/TheHeader.vue'
import Column from '@/components/Column.vue'
import Row from '@/components/Row.vue'
import Keyboard from '@/components/Keyboard.vue'
import Button from '@/components/Button.vue'
import Overlay from '@/components/Overlay.vue'
import ToggleButton from '@/components/ToggleButton.vue'

import iconHelp from '@/assets/icons/help.png'
import iconRestart from '@/assets/icons/restart.png'
import iconSetting from '@/assets/icons/setting.png'
import iconStats from '@/assets/icons/stats.png'

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
const ICONS_FOLDER_PATH = '../assets/icons/'

// ---- BOARD ----
const maxColumnNum = ref(5)
const maxRowNum = ref(5)
const rowIndex = ref(0)
const columnIndex = ref(0)
const rowValues = ref([])
const foundLetters = ref([])

const isCorrect = ref([])
const isWrong = ref([])
const isNotRightPos = ref([])

rowValues.value = createTwoDimArr(maxRowNum.value, maxColumnNum, '')
isCorrect.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isWrong.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)
isNotRightPos.value = createTwoDimArr(maxRowNum.value, maxColumnNum, false)


// ---- SETTINGS ----
const darkModeEnabled = ref(false)
const hardModeEnabled = ref(false)

// ---- GAME STATE ----
const isPlaying = ref(true)
const gameWon = ref(false)
const gameLost = ref(false)

const settingsHidden = ref(true)
const statsHidden = ref(true)

let tryCounts = 0

var currentWord = []
var currentWordString = ""

// ---- COOKIES ----
const COOKIE_N_GAMES_PLAYED = "gamesPlayed"
const COOKIE_N_GAMES_WON = "gamesWon"
const COOKIE_N_BEST_STREAK = "bestStreak"
const COOKIE_N_CURRENT_STREAK = "currentStreak"
const COOKIE_N_BEST_TRY = "bestTry"
const COOKIE_USED_WORDS = "usedWords"

const gamesPlayed = ref(0)
const gamesWon = ref(0)
const bestStreak = ref(0)
const currentStreak = ref(0)
const bestTry = ref(0)

let usedWords = []

//FUNCTIONS =============================================================================

// ---- TOOLS ----
function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function createTwoDimArr(rowNum, columnNum, initValue) {
  let arr = []
  for(let i = 0; i < rowNum; i++) {
    arr[i] = []
    for(let j = 0; j < columnNum; j++) {
      arr[i][j] = initValue
    } 
  }
  return arr
}

function clearRow(rowIndex) {
  for(let i = 0; i < maxColumnNum.value; i++) {
    rowValues.value[rowIndex][i] = ''
    columnIndex.value = 0
  }
}

// ---- ACTIONS ----
function darkModeChanged(state) {
  darkModeEnabled.value = state
}

function hardModeChanged(state) {
  hardModeEnabled.value = state
}

function openSettings () {
  settingsHidden.value = false
  isPlaying.value = false
}

function closeSettings() {
  settingsHidden.value = true
  isPlaying.value = true
}

function oepnStats() {
  statsHidden.value = false
  isPlaying.value = false
}

function closeStats() {
  statsHidden.value = true
  isPlaying.value = true
}

function newGameInit() {
  
  if(usedWords.length === words.length) {
    alert("Congrats, you have finished our database of words :)")
    usedWords = []
    updateCookies()
  }
  while(true) {
    currentWordString = words[getRandomInt(0, words.length - 1)].toUpperCase()
    if(usedWords.includes(currentWordString)) { continue }
    break
  }
   
  const newWordArr = [...currentWordString]

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
  isPlaying.value = true

  tryCounts = 0

  console.log(currentWord)
}

//EVENTS ================================================================================
document.addEventListener('keydown', (e) => {
  
  const val = e.key.toUpperCase()

  if(isPlaying.value) {
    if(val === "ENTER") {
    
      if(columnIndex.value === maxColumnNum.value) {
        if(hardModeEnabled.value) {
          if(!foundLetters.value.every(i => rowValues.value[rowIndex.value].includes(i))) {
            alert('HARD MODE')        
            clearRow(rowIndex.value)
            return
          }
        } 
        tryCounts++
        if(tryCounts === 1) { gamesPlayed.value++ }
        let columsState = [0, 0, 0, 0, 0]
        let rightAnswers = 0

        for(let i = 0; i < 5; i++) {
          
          if(currentWord.includes(rowValues.value[rowIndex.value][i])) { 
            columsState[i] = 1
            if(!foundLetters.value.includes(rowValues.value[rowIndex.value][i])) {
              foundLetters.value.push(rowValues.value[rowIndex.value][i])
            }
          }
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
          usedWords.push(currentWordString)
          gamesWon.value++
          currentStreak.value++
          if(bestStreak.value < currentStreak.value) { bestStreak.value = currentStreak.value }
          gameWon.value = true
          isPlaying.value = false

          if(bestTry.value > rowIndex.value) {
            bestTry.value = rowIndex.value }
        }
        else if(rowIndex.value === maxRowNum.value) {
          usedWords.push(currentWordString)
          gameLost.value = true
          isPlaying.value = false
          currentStreak.value = 0
        }
      }
    }
    else if(val === "BACKSPACE") {
      columnIndex.value--
      rowValues.value[rowIndex.value][columnIndex.value] = ''
    }
    else if(columnIndex.value === maxColumnNum.value) { return }
    else if(val.length === 1 && (/[A-Z]/).test(val)) {
      rowValues.value[rowIndex.value][columnIndex.value] = val
      columnIndex.value++
    }
  }
  if(val === "ESCAPE") {
    newGameInit()
  }
  
})

window.onbeforeunload = function() {
  updateCookies()
  return null
}
//COOKIES ===============================================================================
const COOKIE_USEDWORDS = "usedWords"

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

function getCookiesData() {
  gamesPlayed.value = getCookie(COOKIE_N_GAMES_PLAYED)
  gamesWon.value = getCookie(COOKIE_N_GAMES_WON)
  bestStreak.value = getCookie(COOKIE_N_BEST_STREAK)
  currentStreak.value = getCookie(COOKIE_N_CURRENT_STREAK)
  bestTry.value  = getCookie(COOKIE_N_BEST_TRY)

  usedWords = JSON.parse(getCookie(COOKIE_USED_WORDS))
}
function updateCookies() {
  setCookie(COOKIE_N_GAMES_PLAYED, gamesPlayed.value, 1)
  setCookie(COOKIE_N_GAMES_WON, gameWon.value, 1)
  setCookie(COOKIE_N_BEST_STREAK, bestStreak.value, 1)
  setCookie(COOKIE_N_CURRENT_STREAK, currentStreak.value, 1)
  setCookie(COOKIE_N_BEST_TRY, bestTry.value, 1)

  setCookie(COOKIE_USED_WORDS, JSON.stringify({...usedWords}), 1)
}

function resetCookies() {
  setCookie(COOKIE_N_GAMES_PLAYED, 0, 1)
  setCookie(COOKIE_N_GAMES_WON, 0, 1)
  setCookie(COOKIE_N_BEST_STREAK, 0, 1)
  setCookie(COOKIE_N_CURRENT_STREAK, 0, 1)
  setCookie(COOKIE_N_BEST_TRY, maxRowNum.value + 1, 1)

  setCookie(COOKIE_USED_WORDS, JSON.stringify([]), 1)
}
// MAIN =================================================================================

resetCookies()
getCookiesData()
newGameInit()

</script>

<template>
  <div id="app">
    <TheHeader :dark-mode="darkModeEnabled"></TheHeader>

    <main :class="['main', { 'main--darkmode' : darkModeEnabled }]">
      
      <Overlay :hidden="statsHidden" @background-clicked="closeStats">
        <div class="stats">
          <div class="stats__section-1">
            <div class="stats__box">
              <p class="stats__box__title">Games Played</p>
              <p class="stats__box__value">{{ gamesPlayed }}</p>
            </div>
            <div class="stats__box">
              <p class="stats__box__title">Games Won</p>
              <p class="stats__box__value">{{ gamesWon }}</p>
            </div>
            <div class="stats__box">
              <p class="stats__box__title">Best Streak</p>
              <p class="stats__box__value">{{ bestStreak }}</p>
            </div>
            <div class="stats__box">
              <p class="stats__box__title">Current Streak</p>
              <p class="stats__box__value">{{ currentStreak }}</p>
            </div>
          </div>
        </div>
      </Overlay>

      <Overlay @background-clicked="closeSettings" :hidden="settingsHidden">
        <div class="settings">
          
          
          <div class="settings__section">
            <p class="settings__section__p">Hard Mode</p>  
            <ToggleButton @toggled="hardModeChanged"></ToggleButton>
          </div>
        </div>
      </Overlay>

      <Overlay :hidden="!gameWon">
        <div class="you-won">
          <div class="you-won__info">
            <p class="you-won__message">You Won</p>
            <p class="you-won__the-word"> The Word:&nbsp;{{ currentWordString }}</p>
          </div>
          <div class="you-won__nav">
            <Button @clicked="newGameInit" :is-icon-only="true" icon-name="restart.png" :has-rotaion-animation="true"></Button>
          </div>
        </div>
      </Overlay>

      <Overlay :hidden="!gameLost">
        <div class="you-lost">
          <div class="you-lost__info">
            <p class="you-lost__message">You Lost</p>  
            <p class="you-lost__the-word"> The Word:&nbsp;{{ currentWordString }}</p>
          </div>
          <div class="you-lost__nav">
            <Button @clicked="newGameInit" :is-icon-only="true" icon-name="restart.png" :has-rotaion-animation="true"></Button>
          </div>
        </div>
      </Overlay>
      
      <div class="main__nav">
        <Button @clicked="newGameInit" :is-icon-only="true" :icon="iconRestart" :has-rotaion-animation="true"></Button>
        <Button @clicked="openSettings" :isIconOnly="true" :icon="iconSetting" :has-rotaion-animation="true"></Button>
        <Button @clicked="oepnStats" :is-icon-only="true" :icon="iconStats" :has-mirror-animation="true"></Button>
       
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
  </div>
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
    flex-direction: column;
    justify-content: center;
    align-items: center;

    width: 100%;
    .settings__section {
      display: flex;
      justify-content: space-between;
      align-items: center;

      margin: 1rem 0;
      padding: 1rem;
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
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    padding: 2rem;
    
    height: 100%;
    width: 100%;

    .you-won__info {  
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
  
      .you-won__message {
        color: 1E4652;
        text-transform: uppercase;

        font-size: 3rem;
        font-weight: bold;
      }

      .you-won__the-word {
        color: #1E4652;

        font-size: 2.9rem;
      }
    }

    .you-won__nav {
      display: flex;
      justify-content: flex-start;
      align-items: center;

      width: 100%;
    }
  }

  .you-lost {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    padding: 2rem;
    
    height: 100%;
    width: 100%;

    .you-lost__info {  
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
  
      .you-lost__message {
        color: 1E4652;
        text-transform: uppercase;

        font-size: 3rem;
        font-weight: bold;
      }

      .you-lost__the-word {
        color: #1E4652;

        font-size: 2.9rem;
      }
    }

    .you-lost__nav {
      display: flex;
      justify-content: flex-start;
      align-items: center;

      width: 100%;
    }
  }

  .stats {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;

    padding: 2rem;
    
    height: 100%;
    width: 100%;

    .stats__section-1 {
      display: flex;
      justify-content: center;
      flex-wrap:  wrap;
      .stats__box {
        background-color: #ffffff;

        display: flex;
        flex-direction: column;
        align-items: center;

        border-radius: 15px;
        padding: 1rem;
        margin: .5rem;
        width: 40%;

        .stats__box__title {
          color: 1E4652;
          font-size: 1.2rem;
          font-weight: 500;
        }

        .stats__box__value {
          color: 1E4652;
          font-size: 1.2rem;
          font-weight: 500;
        }
      }
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

      width: 20rem;
      padding: 1rem;
    }
  }
  .main--darkmode {
    background-color: #4A6A74;
  }
</style>
