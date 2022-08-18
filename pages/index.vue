<template>
  <div class="fixed w-full h-full flex flex-col bg-black text-white">
    <div class="flex text-2xl items-center mx-auto mt-8">
      Your Credit:
      <div class="text-4xl ml-5 font-bold">
        {{ credit }}
      </div>
    </div>
    <div class="flex my-auto">
      <div class="grid grid-cols-3 w-4/6 shadow ml-auto">
        <slotCard
          :image="symboles[result[0]].img"
          :roll="slotCard1Roll"
        />
        <slotCard
          :image="symboles[result[1]].img"
          :roll="slotCard2Roll"
        />
        <slotCard
          :image="symboles[result[2]].img"
          :roll="slotCard3Roll"
        />
      </div>
      <div
        class="flex w-28 h-28 items-center justify-center rounded-full text-white text-lg font-bold mr-auto ml-20 my-auto cursor-pointer transition-colors"
        :class="(credit > 0 && !slotCard3Roll) ? 'bg-red-600 hover:bg-red-700' : 'bg-gray-400'"
        @click="roll"
      >
        ROLL
      </div>
    </div>
    <div class="relative flex h-40">
      <div
        ref="cashOut"
        class="absolute top-0 w-1/6 h-12 flex justify-center items-center bg-yellow-500 hover:bg-yellow-600 text-white text-xl rounded-full cursor-pointer transition-colors"
        @mouseenter="randomMoveCashOut"
        @click="getCredits"
      >
        CASH OUT
      </div>
    </div>
  </div>
</template>

<script>
import slotCard from '../components/slot-card.vue'
export default {
  name: 'IndexPage',
  components: { slotCard },
  data () {
    return {
      symboles: [
        { name: 'Cherry', credits: 10, img: 'cherry.svg' },
        { name: 'Lemon', credits: 20, img: 'lemon.svg' },
        { name: 'Orange', credits: 30, img: 'orange.svg' },
        { name: 'WaterMelon', credits: 40, img: 'watermelon.svg' }
      ],
      credit: 10,
      winCredit: 0,
      result: [0, 1, 2],
      slotCard1Roll: false,
      slotCard2Roll: false,
      slotCard3Roll: false,
      canGetCredit: true
    }
  },
  mounted () {
    this.$refs.cashOut.style.left = (window.innerWidth / 2) - (this.$refs.cashOut.clientWidth / 2) + 'px'
  },
  methods: {
    roll () {
      if (this.credit > 0 && !this.slotCard3Roll) {
        this.rollSlots()
        this.credit--
        this.get3RandomSymbole()
        if (this.result[0] === this.result[1] && this.result[1] === this.result[2]) {
          if (this.credit < 40) {
            if (this.result[0] === this.result[1] && this.result[1] === this.result[2]) {
              this.winCredit += this.symboles[this.result[0]].credits
            }
          } else if (this.credit > 40 && this.credit < 60) {
            const chance = this.getRandomInt(1, 101)
            if (chance < 31) {
              this.get3RandomSymbole()
              if (this.result[0] === this.result[1] && this.result[1] === this.result[2]) {
                this.winCredit += this.symboles[this.result[0]].credits
              }
            }
          } else if (this.credit > 60) {
            const chance = this.getRandomInt(1, 101)
            if (chance < 61) {
              this.get3RandomSymbole()
              if (this.result[0] === this.result[1] && this.result[1] === this.result[2]) {
                this.winCredit += this.symboles[this.result[0]].credits
              }
            }
          }
        }
      }
    },
    get3RandomSymbole () {
      for (let i = 0; i < 3; i++) {
        this.result[i] = this.getRandomInt(0, 4)
      }
    },
    rollSlots () {
      this.slotCard1Roll = true
      this.slotCard2Roll = true
      this.slotCard3Roll = true
      setTimeout(() => {
        this.slotCard1Roll = false
      }, 1000)
      setTimeout(() => {
        this.slotCard2Roll = false
      }, 2000)
      setTimeout(() => {
        this.slotCard3Roll = false
      }, 3000)
    },
    getRandomInt (min, max) {
      min = Math.ceil(min)
      max = Math.floor(max)
      return Math.floor(Math.random() * (max - min) + min)
    },
    getCredits () {
      const chance = this.getRandomInt(1, 101)
      if (chance > 40) {
        this.credit += this.winCredit
        this.winCredit = 0
      }
    },
    randomMoveCashOut () {
      const chance = this.getRandomInt(1, 101)
      if (chance <= 50) {
        const direction = this.getRandomInt(0, 2)
        if (direction === 0 && (this.$refs.cashOut.offsetLeft + this.$refs.cashOut.clientWidth) < window.innerWidth) {
          this.$refs.cashOut.style.left = this.$refs.cashOut.offsetLeft + 300 + 'px'
        } else if (direction === 1 && this.$refs.cashOut.offsetLeft > this.$refs.cashOut.clientWidth) {
          this.$refs.cashOut.style.left = this.$refs.cashOut.offsetLeft - 300 + 'px'
        }
      }
    }
  }
}
</script>

<style scoped>
  * {
    font-family: 'Poppins';
  }
</style>
