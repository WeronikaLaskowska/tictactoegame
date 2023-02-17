<template>
  <div v-if="gameWon" class="container" id="play-again">
    <h2 style="text-transform:uppercase;" id="turn" class="neon" data-text="x's turn!">{{ curr }} is a WINNER</h2>
    <a @click="startNewGame" class="btn neon-button">PLAY AGAIN</a>
  </div>
  <div v-if="gameTied" class="container" id="play-again">
    <h2 id="turn" class="neon" data-text="x's turn!">TIE!</h2>
    <a @click="startNewGame" class="btn neon-button">PLAY AGAIN</a>
  </div>
  <h1 style="text-transform:uppercase;" v-if="!gameWon && !gameTied" id="turn" class="neon">
    {{ curr }}'s turn!
  </h1>
  <div v-if="!gameWon && !gameTied" class="board">
    <div @click="onNewChoice" class="field" id="0"></div>
    <div @click="onNewChoice" class="field" id="1"></div>
    <div @click="onNewChoice" class="field" id="2"></div>
    <div @click="onNewChoice" class="field" id="3"></div>
    <div @click="onNewChoice" class="field" id="4"></div>
    <div @click="onNewChoice" class="field" id="5"></div>
    <div @click="onNewChoice" class="field" id="6"></div>
    <div @click="onNewChoice" class="field" id="7"></div>
    <div @click="onNewChoice" class="field" id="8"></div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  setup() {
    let curr = ref(" x");
    let noW = ref("");
    let cnt = ref(0);
    let board = ref(["", "", "", "", "", "", "", "", ""]);
    let gameWon = ref(false);
    let gameTied = ref(false);
    const switchCurr = () => {
      if (curr.value == " x") {
        curr.value = " o";
      } else if (curr.value == " o") {
        curr.value = " x";
      }
    };

    function isTie() {
      let flag = true;
      board.value.forEach((field: string) => {
        if (field === "") flag = false;
      });

      return flag;
    }

    const isWinner = (type: string) => {
      //Poziome
      if (
        (board.value[0] === type &&
          board.value[1] === type &&
          board.value[2] === type) ||
        (board.value[3] === type &&
          board.value[4] === type &&
          board.value[5] === type) ||
        (board.value[6] === type &&
          board.value[7] === type &&
          board.value[8] === type)
      )
        return true;
      //Poziome

      //Pionowe
      if (
        (board.value[0] === type &&
          board.value[3] === type &&
          board.value[6] === type) ||
        (board.value[1] === type &&
          board.value[4] === type &&
          board.value[7] === type) ||
        (board.value[2] === type &&
          board.value[5] === type &&
          board.value[8] === type)
      )
        return true;
      //Pionowe

      //Przekatne
      if (
        (board.value[0] === type &&
          board.value[4] === type &&
          board.value[8] === type) ||
        (board.value[2] === type &&
          board.value[4] === type &&
          board.value[6] === type)
      )
        return true;
      //Przekatne
    };

    const onNewChoice = (e: any) => {
      console.log(e.target.id);
      if (e.target.classList == "field") {
        board.value[e.target.id] = curr.value;
        e.target.classList += curr.value;
        cnt.value = 0;
        if (!isTie() && !isWinner(curr.value)) {
          switchCurr();
        } else if (isWinner(curr.value)) {
          gameWon.value = true;
        } else if (isTie()) {
          gameTied.value = true;
        }
      }
    };
    const startNewGame = () => {
      gameWon.value = false;
      gameTied.value = false;
      board.value = ["", "", "", "", "", "", "", "", ""]
    };
    return { curr, onNewChoice, gameWon, gameTied, startNewGame };
  },
});
</script>

<style scoped></style>
