<template>
  <div id="content" class="bingo-content notouch py-5">
    <div class="bingo-cards-wrapper" v-if="answers.length > 0">
      <div class="bingo-grid-row" v-bind:key="y" v-for="y in tableHeight">
        <div class="bingo-card" @click="answerClick(answers[(y - 1)*tableHeight + (x-1)])" :class="isActive(answers[(y - 1)*tableHeight + (x-1)]) ? 'active' : ''" v-bind:key="x" v-for="x in tableWidth">
          <div class="bingo-card-content">{{ answers[(y - 1)*tableHeight + (x-1)].answer }}</div>
        </div>
      </div>
    </div>

    <div class="reset mt-5">
      <button type="button" class="btn btn-bingo mx-auto d-block" @click="newGame">
        Új ebéd!
      </button>
    </div>
  </div>
</template>

<script>
import BingoElements from '../assets/items'

export default {
  mounted() {
    this.newGame()
  },
  data() {
    return {
      tableWidth: 5,
      tableHeight: 5,
      answers: [],
    }
  },
  methods: {
    isActive(element) {
      return element.count > 0
    },
    answerClick(element) {
      element.count++

      if (this.isBingo) {
        alert('Egészségetekre, hajrá Magyarország, hajrá Magyarok!')
      }
    },
    newGame() {
      const bingoAnswers = BingoElements.answers
      this.answers = this._.shuffle(bingoAnswers).slice(0, this.tableHeight * this.tableWidth).map((element) => {
        return {
          count: 0,
          answer: element
        }
      })
    },
  },
  computed: {
    isBingo() {
      let bingo = true

      this.answers.forEach((elem) => {
        if (elem.count === 0) {
          bingo = false
        }
      })

      return bingo
    }
  }
}
</script>

<style>
.bingo-cards-wrapper {
  padding: 5px;
  margin: 0 auto;
  z-index: 2;
  position: relative;
  overflow: hidden;
}

.bingo-grid-row {
  position: relative;
  display: flex;
}

.bingo-card {
  margin: 5px;
  cursor: pointer;
  text-align: center;
  position: relative;
  background-color: hsla(0, 0%, 100%, 0.95);
  background-image: linear-gradient(0deg, hsla(0, 0%, 70.6%, .8), hsla(0, 0%, 100%, 0.25));
  border: 5px solid #c00;
  font-weight: bold;
  font-family: Lucida Sans Unicode, Lucida Grande, sans-serif;
  font-size: 18px;
  word-wrap: break-word;
  background-repeat: no-repeat;
  background-position: 50%;
  background-size: cover;
  padding: 5px 10px;
  color: #333;
  flex: 1 1 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bingo-card:hover {
  border: 5px solid #e00;
  color: #111;
}

.bingo-card.active {
  background-color: hsla(0, 0%, 66.7%, 0.85);
  border-color: #f22;
  animation-name: bingo;
  animation-duration: 2s;
  animation-iteration-count: infinite;
}

.reset {
  z-index: 3;
}

.btn-bingo {
  border-color: #eee;
  background-color: #cc0000;
  color: #eee;
  font-weight: bold;
}

.btn-bingo:focus {
  box-shadow: none;
}

.btn-bingo:hover {
  color: #eee;
  background-color: #ee0000;
}

@keyframes bingo {
  0% {
    border-color: #ff2222;
  }

  25% {
    border-color: #ee0000;
  }

  50% {
    border-color: #ff9911;
  }

  100% {
    border-color: #ff2222;
  }
}

@media screen and (max-width: 767px) {
  .bingo-grid-row {
    display: block !important;
  }
}
</style>
