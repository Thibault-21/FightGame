<template>
  <section>
    <section class="row-1">
      <div class="small-6 large-4 columns">
        <h2 class="text-center"> ME</h2>
        <div class="healthbar">
          <div 
          class="healthbar text-center"
          style="background-color: blue; margin: 5px; color: white;"
          :style="{width: playerHealth + '%'}"> {{playerHealth}}
          </div>
        </div>
      </div>
      <div class="small-6 large-4 columns">
        <h2 class="text-center"> Dark Vador</h2>
        <div class="healthbar">
          <div 
          class="healthbar text-center"
          style="background-color: red; margin: 5px; color: white;"
          :style="{width: DarkVador + '%'}"> {{DarkVador}}
          </div>
        </div>
      </div>
    </section> 
    <section class="row-2" v-if="!gameIsRunning">
      <div class="small-6">
        <button class="btn btn-st" @click="start">START A NEW GAME</button>
      </div>
    </section>
    <section class="row-3" v-else>
      <button class="btn btn-a" @click="attack">Attack</button>
      <button class="btn btn-s" @click="SpecialAttack">SpecialAttack</button>
      <button class="btn btn-h" @click="Heal">Heal</button>
      <button class="btn btn-g" @click="giveup">GIVE UP</button>
    </section>
    <section class="row-4 log" v-if="turns.length > 0">
      <div class="small-10" columns>
        <ul>
          <li 
          v-for="value in turns" 
          v-bind:key="value.id"
          >
          {{value.text}}
          </li>
        </ul>
      </div>
    </section>
  </section>
  <!--  -->

</template>

<script>
export default {
  name: 'Game', 
  data(){
    return {
      gameIsRunning: false,
      playerHealth: 100,
      DarkVador: 100, 
      turns: []
    }
  }, 
    methods: {
      start(){
        this.gameIsRunning = true; 
        this.playerHealth = 100;
        this.DarkVador = 100;
      },
      attack() {
        var damage = this.calculateDamage(3, 7);
        this.DarkVador -= damage;
        this.turns.unshift({
          isPlayer: true,
          text: "You hit DarkVador for" +  " " + damage
        })
        if(this.checkWin()){
          return;
        }
        this.DarkVadorAttack()
      }, 
      SpecialAttack(){
        var damage = this.calculateDamage(10, 20);
        this.DarkVador -= damage;
        this.turns.unshift({
          isPlayer: true,
          text: "You hit DarkVador for" +  " " + damage
        })
         if(this.checkWin()){
          return;
        }

        this.DarkVadorAttack()
      },
      Heal(){
        if(this.playerHealth <= 90){
          this.playerHealth += 10;
        }else {
          this.playerHealth = 100;
        }
        this.turns.unshift({
          isPlayer: true,
          text: "You heal for" +  " " + this.heal()
        })
      },
      giveup(){
        this.gameIsRunning = false;
      },
      calculateDamage(min, max){
        return Math.max(Math.floor(Math.random() * max) + 1, min)
      }, 
      checkWin(){
        if(this.DarkVador <= 0){
          if(confirm('You won! New Game ?')){
            this.start();
          }else{
            this.gameIsRunning = false;
          }
          return true;
        } else if(this.playerHealth <= 0){
            if(confirm('You lost! New Game?')){
              this.start();
            }else {
              this.gameIsRunning = false;
            }
            return true;
        }
        return false;
      }, 
      DarkVadorAttack(){
        var damage = this.calculateDamage(7, 14);
        this.playerHealth -= damage;
        this.checkWin(); 
         this.turns.unshift({
          isPlayer: false,
          text: "DarkVador hit you for" + " " + damage
        })
      }
    }
  }

</script>

<style scoped>

.text-center {
  text-align: center;
}
.healthbar {
  width: 80%;
  height: 46px;
  background-color: rgb(201, 201, 201);
  border-radius: 10px;
  margin: auto;
  transition: width 500ms;
}
.btn {
  background-color: rgb(186, 233, 186);
  color: black;
  border-radius: 5px;
  height: 40px;
  margin: 20px;
  border: 1px solid rgb(186, 233, 186);
}
:focus {
  outline: none;
}
li {
  list-style-type: none;

}
.btn-a{
  background-color: rgb(147, 211, 161) ;
  border: 1px solid rgb(147, 211, 161);
}
.btn-s{
  background-color: rgb(71, 66, 92);
  border: 1px solid rgb(71, 66, 92);
  color: white;
}
.btn-h{
  background-color: rgb(201, 201, 201);
  border: 1px solid rgb(201, 201, 201);
}
.btn-g{
  background-color: rgb(88, 116, 173);
  border: 1px solid rgb(88, 116, 173);
  color: white;
}
.row-3 {
  margin: 15px;
  border-radius: 5px;
  border: 1px solid lightgrey;
}
.row-4 {
 margin: 15px;
  border-radius: 5px;
  border: 1px solid lightgrey; 
}
.player-turn {
background-color: blue;
color: white;
}
.darkVador-turn {
background-color: red;
color: white;
}
</style>
