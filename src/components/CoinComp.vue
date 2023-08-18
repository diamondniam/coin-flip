<template>
  <div class="coin">
    <div class="tails"></div>
    <div class="heads"></div>
    <div class="head-back"></div>
    <div class="coin-vertical-inner"></div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
  .coin {
    background: #1f1f1f;
    width: 5em;
    height: 5em;
    border-radius: 50%;
    position: absolute;
    transform-style: preserve-3d;
    left: calc(50% - 2.5em);
    transform: translateY(1000px);
  }

  .coin::before {
    background-color: #1f1f1f;
    position: absolute;
    border-radius: 50%;
    content: '';
    height: 5em;
    width: 5em;
    transform: translateZ(-0.75em);
  }

  .coin::after {
    background-color: #1f1f1f;
    content: '';
    left: calc(4.13em - 2.5em);
    position: absolute;
    height: 5em;
    width: .7em;
    transform: rotateZ(90deg) rotateY(90deg) translateY(-.5em) translateX(.4em);
  }

  .coin-vertical-inner {
    background-color: #1f1f1f;
    content: '';
    left: 2.2em;
    position: absolute;
    height: 5em;
    width: .7em;
    transform: rotateY(90deg) translateY(-0em) translateX(.4em);
  }

  .tails{
    box-sizing: border-box;
    background-color: #e7e7e7;
    background-image: url('@/assets/icons8-laurel-wreath-100.png');
    background-repeat: no-repeat;
    background-size: 50%;
    background-position: center;
    position: absolute;
    border-radius: 50%;
    content: '';
    height: 5em;
    width: 5em;
    transform: translateZ(0.001em);
    border: 2px solid #1f1f1f;
  }

  .heads {
    box-sizing: border-box;
    background-color: #e7e7e7;
    background-image: url('@/assets/icons8-female-user-144.png');
    background-repeat: no-repeat;
    background-size: 60%;
    background-position: center;
    position: absolute;
    border-radius: 50%;
    content: '';
    height: 5em;
    width: 5em;
    transform: translateZ(-0.755em);
    border: 2px solid #1f1f1f;
  }

  .head-back {
    background-color: #1f1f1f;
    position: absolute;
    border-radius: 50%;
    content: '';
    height: 5em;
    width: 5em;
    transform: translateZ(-0.01em);
  }

  .spin {
    transform: rotateX(180deg);
  }
</style>

<style>
  @keyframes tails {
    0%   { transform: translateY(1000px) rotateX(0deg) rotateY(240deg) }
    50%  { transform: translateY(0px) rotateX(2880deg) rotateY(10deg) }
    100% { transform: translateY(1000px) rotateX(0deg) rotateY(-120deg) }
  }

  @keyframes heads {
    0%   { transform: translateY(1000px) rotateX(0deg) rotateY(120deg) }
    50%  { transform: translateY(0px) rotateX(2700deg) rotateY(22deg) }
    100% { transform: translateY(1000px) rotateX(0deg) rotateY(120deg) }
  }
</style>