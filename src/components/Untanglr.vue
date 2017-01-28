<template xmlns:v-on="http://www.w3.org/1999/xhtml">
    <div style="margin-top:50px;text-align: center" class="jumbotron">
        <h1>The Hobbit's Word Game</h1>
        <button class="btn btn-primary btn-lg" v-if="!started" v-on:click="startGame">Start Game</button>
        <div v-if="started">
            <p>Guess the following word: <b>{{tangledWord}}</b></p>
            <div class="row">
                <div class="col-sm-6 col-sm-offset-3">
                    <input type="text" v-model="guess" v-on:keyup="keyPressed" class="form-control input-lg"/>
                </div>
            </div>
            <h2>Time Left: {{countdown}}</h2>
        </div>
        <h2 class="alert alert-danger" v-if="isGameOver">Game Over</h2>
        <h3 class="text-center">Your points: {{ points}}</h3>
    </div>
</template>
<script>
    export default {
        name: "Untanglr",
        data() {
            return {
                tangledWord: "izazp",
                correctWord: "pizza",
                guess: "",
                failures: 0,
                points: 0,
                started: false,
                isGameOver: false,
                countdown: 40,
                interval: {}
            }
        },
        methods: {
            keyPressed() {
                let length = this.guess.length;
                if (this.correctWord.split('')[length - 1] != this.guess.split('')[length - 1]) {
                    this.failures++;
                }
                else if (this.correctWord === this.guess) {
                    this.points += Math.floor(Math.pow(1.95, (this.correctWord.length / 3))) - this.failures;
                    this.failures = 0;
                    this.guess = "";
                }
            },
            startGame() {
                this.countdown = 40;
                this.isGameOver = false;
                this.started = true;
                this.interval = setInterval(this.decreaseCountdown, 1000);
            },
            gameOver() {
                this.started = false;
                this.isGameOver = true;
            },
            decreaseCountdown() {
                if (this.countdown == 0) {
                    this.gameOver();
                    clearInterval(this.interval);
                }
                this.countdown--;
            },
            getNextTangledWord() {
                let tangledWords = [
                    {tangledWord: "izazp",  untangledWord: "pizza"}
                ];
            }
        }
    }
</script>
