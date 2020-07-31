<template>
 <!DOCTYPE html>
  <div id="app">
      <section class="row" v-if="isGameStarted">
          <div class="small-6 columns">
              <h1 class="text-center">YOU</h1>
              <div class="healthbar">
                  <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" v-bind:style="{width:playerHealth+'%'}">
                    {{playerHealth}}
                  </div>
              </div>
          </div>
          <div class="small-6 columns">
              <h1 class="text-center">MONSTER</h1>
              <div class="healthbar">
                  <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" v-bind:style="{width:monsterHealth+'%'}">
                    {{monsterHealth}}
                  </div>
              </div>
          </div>
      </section>
      <section class="row controls" v-if="!isGameStarted">
          <div class="small-12 columns">
              <button v-on:click="setGameToStarted(), initGame()" id="start-game">START NEW GAME</button>
          </div>
      </section>
      <section class="row controls" v-if="isGameStarted">
          <div class="small-12 columns">
              <button v-on:click="setEventHappened(), attack()" id="attack">ATTACK</button>
              <button v-on:click="setEventHappened()" id="special-attack">SPECIAL ATTACK</button>
              <button v-on:click="setEventHappened(), heal()" id="heal">HEAL</button>
              <button v-on:click="setEventHappened(), giveUp()" id="give-up">GIVE UP</button>
          </div>
      </section>
      <section class="row log" v-if="isAttackHappened">
          <div v-for="(log,index) in logs" :key="log" class="small-12 columns">
              <ul>
                  <li v-bind:style="index%2 == 1 ? 'background-color:red; color:white' : 'background-color:green; color:white'">
                    {{log}}
                  </li>
              </ul>
          </div>
      </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function(){
    return {
      isGameStarted: false,
      isAttackHappened: false,
      playerHealth: 100,
      monsterHealth: 100,
      logs: []
    }
  },
  methods: {
    setGameToStarted: function(){
      this.isGameStarted = true
    },
    setEventHappened: function(){
      this.isAttackHappened = true;
    },
    initGame: function(){
      this.playerHealth = 100
      this.monsterHealth = 100
    },
    attack: function(){
      var playerDamage = Math.round(Math.random() * 15)
      var monsterDamage = Math.round(Math.random() * 12)
      this.playerHealth = this.playerHealth - playerDamage
      this.monsterHealth = this.monsterHealth - monsterDamage
      this.logs.push("Player inflicted " + monsterDamage + " damage")
      this.logs.push("Monster inflicted " + playerDamage + " damage")
      this.isGameFinished();
    },
    heal: function(){
      var playerHeal = Math.round(Math.random() * 20)
      var playerDamage = Math.round(Math.random() * 15)
      this.playerHealth = this.playerHealth + playerHeal - playerDamage
      this.logs.push("Player healed " + playerHeal + " damage")
      this.logs.push("Monster inflicted " + playerDamage + " damage")
      this.isGameFinished()
    },
    giveUp: function(){
      this.initGame();
    },
    isGameFinished: function (){
      if (this.playerHealth <= 0){
        alert("You lose :(")
      }
      else if (this.monsterHealth <= 0){
        alert("You win :D")
      }
    }
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
