<template>
  <main>
    <div class="container position-relative">
      <div class="d-flex justify-content-between align-items-center">
        <div class="blue-score">
          <h4>Blue Score:</h4>
          <p>{{ blueWinCounter }} Wins</p>
        </div>
        <h1 class="text-center fw-bold mt-5" style="font-size: 3.5em;">
          <span style="color:rgb(14, 14, 140)">Conn</span><span style="color:rgb(175, 24, 24)">ect </span><span id="four">4</span>
        </h1>
        <div class="red-score">
          <h4>Red Score:</h4>
          <p>{{ redWinCounter }} Wins</p>
        </div>
      </div>
      <button class="btn btn-dark hide text-light" v-show="hide" @click="hide = false">Show</button>
      <div class="win" :class="{'d-none': hide}" v-show="gameEnd">
        <h1 v-if="gameWinner === 1 || gameWinner === 0" class="text-success fw-bold p-2">Congratulations!</h1>
        <h1 v-else class="text-success fw-bold p-2">Unlucky!</h1>
        <div v-if="gameWinner === 1">
          <h1 class="text-center" style="color:rgb(14, 14, 140)">Blue Wins</h1>
          <p class="moves">With {{blueCounter}} Moves</p>
          <div>
            <button class="btn my-btn" @click="reset">Play Again</button>
            <button class="btn btn-dark text-light" @click="hide = true">Hide</button>
          </div>         
        </div>
        <div v-else-if="gameWinner === 3">
          <h1 class="text-center text-warning">Game Tie</h1>
          <p class="moves">With {{redCounter + blueCounter}} Moves</p>
          <div>
            <button class="btn my-btn" @click="reset">Play Again</button>
            <button class="btn btn-dark text-light" @click="hide = true">Hide</button>
          </div>
        </div>
        <div v-else>
          <h1 class="text-center" style="color:rgb(175, 24, 24)">Red Wins</h1>
          <p class="moves">With {{redCounter}} Moves</p>
          <div>
            <button class="btn my-btn" @click="reset">Play Again</button>
            <button class="btn btn-dark text-light" @click="hide = true">Hide</button>
          </div>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-12 my-div d-flex justify-content-center">
          <div class="my-rows" v-for="(rows, rowsIndex) in biArray" :class="{'pulse': gameEnd === false}" @mouseenter="previewVerticalCheck(rowsIndex)" @mouseleave="removePreview(rowsIndex)" :key="rowsIndex">
            <div class="disk" v-for="(cols, colsIndex) in rows" :key="colsIndex" @click="insertDisk(rowsIndex, colsIndex), previewVerticalCheck(rowsIndex), playSound(), gameWin()"
             :class="{'disk-blue': cols === 1 || cols === 3, 'disk-red': cols === 0 || cols === 2, 'disabled': gameEnd || clicked || cols === 1 || cols === 0, 'disk-blue-preview': cols === 5 && redBlueSwitch && gameEnd === false, 'disk-red-preview': cols === 5 && redBlueSwitch === false && gameEnd === false}" >
             <i v-show="gameEnd && (cols === 3 || cols === 2)" class="fa-solid fa-star text-warning star"></i>
            </div>
          </div>
        </div>
      </div>  
      <div class="row">  
        <div class="turnCard m-auto">
          <div v-if="redBlueSwitch" class="d-flex justify-content-center align-items-center blue">
            <p>Blue Turn </p>
            <div class="disk-blue"></div>
          </div>
          <div class="d-flex justify-content-center align-items-center red" v-else>
            <p>Red Turn </p>
            <div class="disk-red"></div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
  export default {
    name: 'App',
    data(){
      return{
        rows: 8,
        cols: 6,
        biArray: [],
        redBlueSwitch: true,
        gameEnd: false,
        clicked: false,
        gameWinner: null,
        blueCounter: 0,
        redCounter: 0,
        hide: false,
        blueWinCounter: 0,
        redWinCounter: 0,
        diskSound: new Audio('../images/Disk-Sound.mp3')
      }
    },
    methods:{
      /**
       * Initializes a new array with null values.
       * Creates a bi-dimensional array with the specified number of rows and columns.
       */
      arrayCreation(){
        for(let i = 0; i < this.rows; i++){
          this.biArray[i] = []
          for(let j = 0; j < this.cols; j++){
            this.biArray[i][j] = null
          }
        }
      },
      /*
      *
      */
      previewVerticalCheck(x){
        for (let i = this.biArray[x].length - 1; i >= 0; i--) {
          if (this.biArray[x][i] === null) {
            console.log(this.biArray[x][i])
            this.biArray[x][i] = 5
            break;
          }
        }
      },
      /*
      * 
      */
      removePreview(x){
        for (let i = this.biArray[x].length - 1; i >= 0; i--) {
          if (this.biArray[x][i] === 5) {
            console.log(this.biArray[x][i])
            this.biArray[x][i] = null
            break;
          }
        }
      },
      /**
       Adds the given value to the last available slot in the specified column of the bi-dimensional array.
       * If the column is full, the value is not added.
       * Updates the value of the redBlueSwitch property.
       */
      diskVerticalCheck(x, z){
        for (let i = this.biArray[x].length - 1; i >= 0; i--) {
          if (this.biArray[x][i] === null || this.biArray[x][i] === 5) {
            this.biArray[x][i] = z;
            break;
          }
        }
        this.redBlueSwitch = !this.redBlueSwitch;
      },
      /**
       * * Insert a disk into the game board at the specified position.
       * * If the position is already occupied, the disk is not inserted.
       */
      insertDisk(x, y){
        this.clicked = true
        setTimeout(() => {
          this.clicked = false
        }, 470)
        if(this.biArray[x][y] !== 1 && this.biArray[x][y] !== 0){
          if(this.redBlueSwitch){
            this.diskVerticalCheck(x, 1)
            this.blueCounter++
          }else{
            this.diskVerticalCheck(x, 0)
            this.redCounter++
          }
        }    
      },
      playSound(){
        this.diskSound.play()
      },
      /**
       * Check (vertically, horizontally and diagonally) if a player has won the game.
       * If a player has won, the gameEnd property is set to true.
       */
      gameWin(){
        // Check vertically
        for(let i = 0; i < this.rows; i++){
          for(let j = 0; j <= this.cols - 4; j++){
            const disk = this.biArray[i][j]
            if(disk !== null && disk === this.biArray[i][j + 1] && disk === this.biArray[i][j + 2] && disk === this.biArray[i][j + 3]){
              this.gameWinner = disk
              this.endGame()
              if(this.gameWinner === 1){
                this.biArray[i][j] = 3
                this.biArray[i][j + 1] = 3
                this.biArray[i][j + 2] = 3
                this.biArray[i][j + 3] = 3
              }else{
                this.biArray[i][j] = 2
                this.biArray[i][j + 1] = 2
                this.biArray[i][j + 2] = 2
                this.biArray[i][j + 3] = 2
              }
            }
          }
        }
        // Check horizontally
        for (let x = 0; x < this.cols; x++) {
          for (let y = 0; y <= this.rows - 4; y++) {
            const disk = this.biArray[y][x];
            if (disk !== null && disk === this.biArray[y + 1][x] && disk === this.biArray[y + 2][x] && disk === this.biArray[y + 3][x]) {
              this.gameWinner = disk
              this.endGame()
              if(this.gameWinner === 1){
                this.biArray[y][x] = 3
                this.biArray[y + 1][x] = 3
                this.biArray[y + 2][x] = 3
                this.biArray[y + 3][x] = 3
              }else{
                this.biArray[y][x] = 2
                this.biArray[y + 1][x] = 2
                this.biArray[y + 2][x] = 2
                this.biArray[y + 3][x] = 2
              }
            }
          }
        }
         // Check diagonally (top-left to bottom-right)
        for (let z = 0; z <= this.rows - 4; z++) {
          for (let c = 0; c <= this.cols - 4; c++) {
            const disk = this.biArray[z][c];
            if (disk !== null && disk === this.biArray[z + 1][c + 1] && disk === this.biArray[z + 2][c + 2] && disk === this.biArray[z + 3][c + 3]) {
              this.gameWinner = disk
              this.endGame()
              if(this.gameWinner === 1){
                this.biArray[z][c] = 3
                this.biArray[z + 1][c + 1] = 3
                this.biArray[z + 2][c + 2] = 3
                this.biArray[z + 3][c + 3] = 3
              }else{
                this.biArray[z][c] = 2
                this.biArray[z + 1][c + 1] = 2
                this.biArray[z + 2][c + 2] = 2
                this.biArray[z + 3][c + 3] = 2
              }
            }
          }
        }
        // Check diagonally (top-right to bottom-left)
        for (let b = 0; b <= this.rows - 4; b++) {
          for (let v = this.cols - 1; v >= 3; v--) {
            const disk = this.biArray[b][v];
            if (disk !== null && disk === this.biArray[b + 1][v - 1] && disk === this.biArray[b + 2][v - 2] && disk === this.biArray[b + 3][v - 3]) {
              this.gameWinner = disk
              this.endGame()
              if(this.gameWinner === 1){
                this.biArray[b][v] = 3
                this.biArray[b + 1][v - 1] = 3
                this.biArray[b + 2][v - 2] = 3
                this.biArray[b + 3][v - 3] = 3
              }else{
                this.biArray[b][v] = 2
                this.biArray[b + 1][v - 1] = 2
                this.biArray[b + 2][v - 2] = 2
                this.biArray[b + 3][v - 3] = 2
              }
            }
          }
        }
        // Check if the game is a draw
        if(this.blueCounter + this.redCounter === this.rows * this.cols){
          this.endGame()
          this.gameWinner = 3
        }
      },
      /** 
       * End the game.
       * Count the number of wins for each player.
       */
      endGame(){
        this.gameEnd = true
        if(this.gameWinner === 1){
          this.blueWinCounter++
        }else{
          this.redWinCounter++
        }
      },
      // Reset the game.
      reset(){
        this.arrayCreation()
        this.gameEnd = false
        this.gameWinner = null
        this.redCounter = 0
        this.blueCounter = 0
        this.redBlueSwitch = true
        this.hide = false
      }
    },
    created(){
      this.arrayCreation()
    },
  }
</script>

<style lang="scss" scoped>
@mixin yellowishBorder {
  border: 3px solid #dfbc9a;
  border-radius: 0.5em;
  position: relative;
}
@mixin beforeBorder{
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border: 1px solid black;
  border-radius: 0.4em;
}
@mixin afterBorder{
  content: '';
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  border-top: 2px solid #f78a06;
  border-left: 2px solid #f78a06;
  border-bottom: 2px solid #875626;
  border-right: 2px solid #875626;
  border-radius: 0.6em;
  outline: 2px solid black;
}
.my-div{
  width: 55%;
  background-color: #313131;
  margin: 40px 0;
  padding: 10px;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
}
.disk{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    background-color: #0a0b0c;
    margin: 5px;
    position: relative;
  }
.disk-blue{
  width: 75px;
  height: 75px;
  border-radius: 50%;
  border: 5px solid rgb(5, 5, 68);
  background-color: rgb(14, 14, 140);
  margin: 5px;
}
.disk-red{
  width: 75px;
  height: 75px;
  border-radius: 50%;
  border: 5px solid rgb(72, 4, 4);
  background-color: rgb(175, 24, 24);
  margin: 5px;
}
.blue{

  p{
    padding-top: 5px;
    color: rgb(14, 14, 140);
    font-weight: bold;
    font-size: 30px;
  }
}
.red{

  p{
    padding-top: 5px;
    color: rgb(175, 24, 24);
    font-weight: bold;
    font-size: 30px;
  }
}
.turnCard{
  width: 22%;
  @include yellowishBorder;
  background-color: #313131;
  margin: 40px 0;
  padding: 20px;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);

  &:before{
    @include beforeBorder;
  }
  &:after{
    @include afterBorder;
  }
}
.disabled{
  pointer-events: none;
}
.win{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background-color: #313131;
  z-index: 100;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
  transition: all 1s ease;

  div{
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;

    .moves{
      color: #0a0b0c;
      font-size: 1.5em;
      text-align: center;
    }
    .my-btn{
      background-color: greenyellow;
      color: #0a0b0c;
      font-weight: bold;
      font-size: 1.2em;
      margin: 5px;
    }
  }
}
.hide{
      position: absolute !important;
      top: 14%;
      left: 17%;
}
.blue-score{
  margin-top: 20px;
  color: white;
  padding: 10px;
  padding-left: 20px;
  padding-right: 20px;
  background-color: #313131;
  font-weight: bold;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
  @include yellowishBorder;

  &:before{
    @include beforeBorder;
  }
  
  &:after{
    @include afterBorder;
  }

  h4{
    font-size: 1.5em;
    color: rgb(14, 14, 140);
  }
  p{
    text-align: center;
  }
}
.red-score{
  margin-top: 20px;
  color: white;
  padding: 10px;
  padding-left: 20px;
  padding-right: 20px;
  background-color: #313131;
  font-weight: bold;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
  @include yellowishBorder;

  &:before{
    @include beforeBorder;
  }
  
  &:after{
    @include afterBorder;
  }

  h4{
    font-size: 1.5em;
    color: rgb(175, 24, 24);
  }
  p{
    text-align: center;
  }
}
#four{
  color: #0a0b0c;
  -webkit-text-stroke-width: 0.3px;
  -webkit-text-stroke-color: white;
}
.star{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.my-rows{
  padding: 5px 2px;
  border-radius: 1em;

}
.disk-blue-preview{
  width: 75px;
  height: 75px;
  border-radius: 50%;
  border: 5px solid rgb(5, 5, 68);
  background-color: rgb(14, 14, 140);
  margin: 5px;
  opacity: 50%;
}
.disk-red-preview{
  width: 75px;
  height: 75px;
  border-radius: 50%;
  border: 5px solid rgb(72, 4, 4);
  background-color: rgb(175, 24, 24);
  margin: 5px;
  opacity: 50%;
}
.pulse{

  &:hover{
    animation: pulse 2s infinite;
  }
}
@keyframes pulse {
  0% {
    background-color: rgb(79, 149, 79);
  }
  50% {
    background-color: rgb(158, 210, 158);
  }
  100% {
    background-color: rgb(79, 149, 79);
  }
}
</style>
