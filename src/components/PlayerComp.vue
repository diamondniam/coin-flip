<template>
  <div class="player">
    <player-icon :usedIcons="usedIcons"/>

    <div class="name">
      <div>NAME:</div>
      <input 
        type="text"
        v-bind:value="player.name"
        @input="player.name = $event.target.value; $emit('inputEmit', $event.target.value, this.player.id)"
      >
    </div>

    <div class="side">
      <div>SIDE:</div>
      <div class="sides">
        <button class="heads" @click="player.side = 'heads'; outline(); $emit('sideEmit', this.player.side, this.player.id)">HEADS</button>
        <button class="tails" @click="player.side = 'tails'; outline(); $emit('sideEmit', this.player.side, this.player.id)">TAILS</button>
      </div>
    </div>

    <!-- <div class="coins">
      <div>COINS:</div>
      <div>{{ coins }}</div>
    </div> -->
  </div>
</template>

<script>
import PlayerIcon from '@/components/PlayerIcon.vue'

export default {
  components: {
    PlayerIcon
  },
  props: {
    id: Number,
    name: String,
    usedIcons: Array,
    coins: Array
  },
  data() {
    return {
      player: {
        id: this.id,
        name: this.name, 
        side: 'Side was not chosen',
        // coins: this.coins[this.id]
      }
    }
  },
  methods: {
    outline() {
      this.$el.querySelectorAll('.sides *').forEach(element => {
        element.classList.remove('outline')
      })
      this.$el.querySelector(`.${(this.player.side).toLowerCase()}`).classList.add('outline')
    }
  }
}
</script>

<style scoped>
  .player {
    padding: 20px;
    width: 30%;
    height: 400px;
    border-radius: 10px;
  }

  div {
    margin-bottom: 15px;
  }

  input {
    outline: none;
    width: 80%;
    border: none;
    border-bottom: 1px solid #000000;
    background: transparent;
    color: #000000;
    font-size: 25px;
  }

  button {
    margin-right: 20px;
    background: #000;
  }

  button:hover {
    opacity: 1;
  }

  .name, .side {
    margin-bottom: 40px;
  }
</style>