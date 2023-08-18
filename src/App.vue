<template>
  <div class="container">
    <player-comp class="player-1" 
      @side-emit="(side, id) => { 
       players[id].side = side; 
        player_1 = new Player(id,players[id].name,players[id].side);
      }" 
      @input-emit="(val, id) => { 
       players[id].name = val; 
        player_1 = new Player(id,players[id].name,players[id].side);
      }"
      @get-icon="getRandomIcon" :usedIcons="usedIcons"
      v-bind="{ id: 0 }" name="Player#1"/>
    <div class="button">
      <button class="flip"
        @click="sideDetect(); nameDetect(); 
        sideChosen && nameChosen ? gameStart() : alert(); sideChosen && nameChosen ? game.flip() : alert()"
        >FLIP
      </button>
      <div class="underButton"></div>
    </div>
    
    <coin-comp/>

    <player-comp class="player-2" 
      @side-emit="(side, id) => { 
       players[id].side = side; 
        player_2 = new Player(id,players[id].name,players[id].side,players[id].coins);        
      }" 
      @input-emit="(val, id) => { 
       players[id].name = val; 
        player_2 = new Player(id,players[id].name,players[id].side,players[id].coins);
      }"
      @get-icon="getRandomIcon" :usedIcons="usedIcons"
      v-bind="{ id: 1 }" name="Player#2"/>
  </div>
</template>

<script>
import PlayerComp from "@/components/PlayerComp.vue"
import CoinComp from './components/CoinComp.vue'

export default {
  components: {
    PlayerComp,
    CoinComp
  },
  data() {
    return {
      players: [
        { id: 0, name: 'Player#1', side: 'Choose the side', coins: 0 },
        { id: 1, name: 'Player#2', side: 'Choose the side', coins: 0 }
      ],
      sideChosen: false,
      nameChosen: false,
      usedIcons: []
    }
  },
  methods: {
    sideDetect() {
      if (this.players[0].side !== 'Choose the side' && this.players[1].side !== 'Choose the side') {
        this.sideChosen = true
      } else {
        this.sideChosen = false
      }
    },
    nameDetect() {
      if (this.players[0].name !== '' && this.players[1].name !== '') {
        this.nameChosen = true
      } else {
        this.nameChosen = false
      }
    },
    getRandomIcon(icon) {
      this.usedIcons.push(icon)
    }
  }
}
</script>

<script setup>
  class Game {
    player_1
    player_2
    side
    reward_for_win = 50
    reward_for_draw = 25

    constructor (player_1, player_2) {
      this.player_1 = player_1
      this.player_2 = player_2
    }

    flip() {
      document.querySelector('.underButton').innerHTML = ''
      document.querySelector('.button button').style.border = 'none'
      document.querySelector('.flip').classList.add('disableClick')

      const player1 = document.querySelector('.player-1')
      const player2 = document.querySelector('.player-2')

      player1.classList.remove('outline')
      player2.classList.remove('outline')

      document.querySelector('.button button').innerHTML = '.'

      setTimeout(() => {
        document.querySelector('.button button').innerHTML = '..'
        setTimeout(() => {
          document.querySelector('.button button').innerHTML = '...'
          setTimeout(() => {
            document.querySelector('.button button').innerHTML = '..'
            setTimeout(() => {
              document.querySelector('.button button').innerHTML = '.'
            }, 600)
          }, 600)
        }, 600)
      }, 600)

      let num = Math.round(Math.random() * (2 - 1) + 1)

      let animationTime = 8000

      const coin = document.querySelector('.coin')

      if (num === 1) {
        this.side = 'heads'

        coin.style.animation = `heads ${animationTime}ms cubic-bezier(.52,.01,.36,1.13)`

        setTimeout(() => {
          coin.style.animation = `none`
          document.querySelector('.flip').classList.remove('disableClick')
        }, animationTime - 2000)
      } else {
        this.side = 'tails'

        coin.style.animation = `tails ${animationTime}ms cubic-bezier(.52,.01,.36,1.13)`
        setTimeout(() => {
          coin.style.animation = `none`
          document.querySelector('.flip').classList.remove('disableClick')
        }, animationTime - 2000)
      }

      let delay = 1000
      var flip_logs = new Promise(r => {
        // setTimeout(() => {
        //   r(document.querySelector('.underButton').innerHTML = 'Getting side...')
        // }, delay)
        r()
      })
      
      flip_logs.then(data => {
        data = (this.side).toUpperCase()

        setTimeout(() => {
          setTimeout(() => {
            document.querySelector('.button button').innerHTML = 'FLIP'
            document.querySelector('.button button').style.border = '2px solid black'
          }, delay * 3)

          if (this.player_1.side === this.side) {
            // document.querySelector('.underButton').innerHTML = `${this.player_1.name} is winner! You get +${this.reward_for_win} coins`
            this.player_1.coins += this.reward_for_win

            player1.classList.add('outline')
          } 
          
          if (this.player_2.side === this.side) {
            // document.querySelector('.underButton').innerHTML = `${this.player_2.name} is winner! You getting +${this.reward_for_win} coins`
            this.player_2.coins += this.reward_for_win
            player2.classList.add('outline')
          } 
          
          if (this.player_2.side === this.side && this.player_1.side === this.side) {
            // document.querySelector('.underButton').innerHTML = `Draw! You both getting +${this.reward_for_draw} coins`
            this.player_1.coins += this.reward_for_draw
            this.player_2.coins += this.reward_for_draw
            player1.classList.add('outline')
            player2.classList.add('outline')
          }

          if (this.player_2.side !== this.side && this.player_1.side !== this.side) {
          //   document.querySelector('.underButton').innerHTML = `You are both wrong. And do not get a reward :(`
          }
          document.querySelector('.button button').innerHTML = data
        }, animationTime - 4500)
      })
    }
  }

  class Player {
    number; name; side; coins

    constructor(number, name = `player#${number}`, side, coins) {
      this.number = number
      this.name = name
      this.side = side
      this.coins = coins
    }
  }

  let player_1, player_2, game

  function gameStart() {
    game = new Game(player_1, player_2)
  }

  function alert() {
    document.querySelector('.underButton').innerHTML = 'Enter the data!'
  }
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;

    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-size: 30px;
  }

  body {
    overflow: hidden;
  }

  img {
    filter: grayscale(1) brightness(50%);
  }

  .container {
    height: 100vh;
    padding: 100px;
    width: 100%;
    background: #e4e4e4;
    display: flex;
    justify-content: space-around;
    align-items: center;
    color: #0b1322;
  }

  * button {
    cursor: pointer;
    width: 100px;
    border-radius: 5px;
    padding: 10px;
    background: black;
    opacity: .8;
    color: #ffffff;
    border: none;
    transition: opacity .2s;
    font-size: 20px;
  }

  .button {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 300px;
  }

  .button button {
    background: transparent;
    color: black;
    font-size: 30px;
    width: 130px;
    border: 2px solid black;
    margin-bottom: 10px;
  }

  .underButton {
    font-size: 15px;
    text-align: center;
    width: 100px;
  }

  .outline {
    outline: 2px solid rgb(0, 0, 0);
    opacity: 1;
  }

  .disableClick {
    pointer-events: none;
  }

  .player-2 {
    text-align: right;
  }

  .player-2 input {
    text-align: right;
  }

  .player-2 .tails {
    margin-right: 0px;
  }

  .player-2 .icon {
    margin-left: calc(100% - 90px);
  }
</style>
