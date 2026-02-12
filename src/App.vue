<script setup>
import { ref } from 'vue'
import welcomeGif from './assets/tuli.gif'
import sadGif from './assets/turn_sad.gif'
import kissGif from './assets/kiss_lay.gif'
import yayGif from './assets/hearts_confetti.gif'

const count = ref(0)
const showButtons = ref(true)
const showSuccess = ref(false)

const swapGif = (newGif) => {
  const img = document.querySelector('.gif-wrapper .gif')
  if (img) {
    img.src = newGif
  } else {
    const container = document.querySelector('.gif-wrapper')
    if (container) container.innerHTML = `<img src="${newGif}" class="gif" />`
  }
}

const swapGifText = (newText) => {
  const title = document.querySelector('.gif-wrapper h2')
  if (title) {
    title.textContent = newText
  } else {
    const container = document.querySelector('.gif-wrapper')
    if (container) container.insertAdjacentHTML('beforeend', `<h2>${newText}</h2>`)
  }
}

const adjustAnger = (isAdding = true) => {
  const gifEl = document.querySelector('.gif-wrapper')
  const yesBtn = document.querySelector('.button.button-yes')
  const noBtn = document.querySelector('.button.button-no')
  const title = document.querySelector('.gif-wrapper h2')

  const gifScale = Math.pow(1.25, count.value)
  const yesScale = Math.pow(1.15, count.value)
  const noScale = Math.pow(0.85, count.value)

  if (gifEl) gifEl.style.transform = `scale(${gifScale})`
  if (yesBtn) yesBtn.style.transform = `scale(${yesScale})`
  if (noBtn) noBtn.style.transform = `scale(${noScale})`
  if (title) {
    if (isAdding) {
      title.textContent = title.textContent + 'f'.repeat(count.value)
    } else {
      title.textContent = title.textContent.slice(0, -1)
    }
  }
}

const onClickNo = () => {
  swapGif(sadGif)
  swapGifText('pff')
  count.value += 1
  adjustAnger(true)
}

const onClickYes = () => {
  count.value -= 1

  if (count.value >= 0) {
    adjustAnger(false)
  }

  if (count.value === -1) {
    swapGifText('Hihu  :>>>')
    swapGif(kissGif)
    showButtons.value = false
    showSuccess.value = true
  }
}
</script>

<template>
  <div class="wrapper">
    <div class="gif-wrapper">
      <img :src="welcomeGif" alt="Heart Logo" class="gif" />
      <h2>Zostaniesz moją Walentynką? 🥹</h2>
    </div>
    <div class="response-wrapper">
      <div class="buttons" v-if="showButtons">
        <button class="button button-yes" @click="onClickYes">Tak</button>
        <button class="button button-no" @click="onClickNo">Nie</button>
      </div>
      <div class="success" v-if="showSuccess">
        <img :src="yayGif" alt="Yay GIF" class="gif" v-if="showSuccess" />
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  transition: transform 220ms ease;
}
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 100vh;
  gap: 0;
}

.gif-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  padding: 0;
  height: 50vh;
  width: 100%;
}

.response-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 50vh;
  width: 100%;
}

.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
}

.success {
  display: flex;
  align-items: center;
  justify-content: center;
  max-height: 90%;
}

.gif {
  display: flex;
  justify-content: center;
  min-height: 90%;
  max-height: 100%;
  transition: transform 220ms ease;
}

.gif + h2 {
  text-align: center;
  font-size: 2.5rem;
  max-height: 90%;
  color: #333;
  margin: 0;
}

.button {
  margin: 0 0.5rem;
  padding: 2rem 4rem;
  font-size: 2.5rem;
  background-color: #b879ff;
  color: white;
  border: none;
  border-radius: 9999px;
  cursor: pointer;
  transition:
    transform 220ms ease,
    background-color 160ms ease;
}

.button:hover {
  background-color: #bb8aff;
}

/* Mobile adjustments */
@media (max-width: 1221px) {
  .button {
    padding: 4rem 6rem;
    font-size: 4rem;
    margin: 2rem;
  }

  .buttons {
    gap: 2rem;
  }

  .gif {
    min-width: 70%;
    margin-bottom: 2rem;
  }

  .gif + h2 {
    font-size: 4rem;
  }
}
</style>
