<template>
  <main>
    <div class="container position-relative">
      <h1 class="text-center fw-bold mt-5" style="font-size: 3.5em;">
        <span style="color:rgb(14, 14, 140)">Conn</span><span style="color:rgb(175, 24, 24)">ect </span><span style="color: #0a0b0c;">4</span>
      </h1>
      <div class="win" v-show="gameEnd">
        <h1 class="text-success fw-bold p-2">Congratulatios!</h1>
        <div v-if="gameWinner === 1">
          <h1 class="text-center" style="color:rgb(14, 14, 140)">Blue Wins</h1>
          <p class="moves">With {{blueCounter}} Moves</p>
          <button class="btn my-btn" @click="reset">Play Again</button>
        </div>
        <div v-else>
          <h1 class="text-center" style="color:rgb(175, 24, 24)">Red Wins</h1>
          <p class="moves">With {{redCounter}} Moves</p>
          <button class="btn my-btn" @click="reset">Play Again</button>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-12 my-div d-flex justify-content-center">
          <div v-for="(rows, rowsIndex) in biArray" :key="rowsIndex">
            <div class="disk" v-for="(cols, colsIndex) in rows" :key="colsIndex" @click="insertDisk(rowsIndex, colsIndex), gameWin()"
             :class="{'disk-blue': cols === 1, 'disk-red': cols === 0, 'disabled': gameEnd }" ></div>
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
        gameWinner: null,
        blueCounter: 0,
        redCounter: 0,
      }
    },
    methods:{
      arrayCreation(){
        for(let i = 0; i < this.rows; i++){
          this.biArray[i] = []
          for(let j = 0; j < this.cols; j++){
            this.biArray[i][j] = null
          }
        }
        console.log(this.biArray)
      },
      diskVerticalCheck(x, z){
        for (let i = this.biArray[x].length - 1; i >= 0; i--) {
          if (this.biArray[x][i] === null) {
            this.biArray[x][i] = z;
            break;
          }
        }
        this.redBlueSwitch = !this.redBlueSwitch;
        // if(this.biArray[x][this.biArray[x].length - 1] === null){
        //   this.biArray[x][this.biArray[x].length - 1] = z
        // }else if(this.biArray[x][this.biArray[x].length - 2] === null){
        //   this.biArray[x][this.biArray[x].length - 2] = z
        // }else if(this.biArray[x][this.biArray[x].length - 3] === null){
        //   this.biArray[x][this.biArray[x].length - 3] = z
        // }else if(this.biArray[x][this.biArray[x].length - 4] === null){
        //   this.biArray[x][this.biArray[x].length - 4] = z
        // }else if(this.biArray[x][this.biArray[x].length - 5] === null){
        //   this.biArray[x][this.biArray[x].length - 5] = z
        // }else if(this.biArray[x][this.biArray[x].length - 6] === null){
        //   this.biArray[x][this.biArray[x].length - 6] = z
        // }
        // this.redBlueSwitch = !this.redBlueSwitch  
      },  
      insertDisk(x, y){
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
      gameWin(){
        for(let i = 0; i < this.rows; i++){
          for(let j = 0; j <= this.cols - 4; j++){
            const disk = this.biArray[i][j]
            if(disk !== null && disk === this.biArray[i][j + 1] && disk === this.biArray[i][j + 2] && disk === this.biArray[i][j + 3]){
            this.gameWinner = disk
            this.endGame()  
            }
          }
        }
      },
      endGame(){
        this.gameEnd = true
      },
      reset(){
        this.arrayCreation()
        this.gameEnd = false
        this.gameWinner = null
        this.redCounter = 0
        this.blueCounter = 0
        this.redBlueSwitch = true
      }
    },
    created(){
      this.arrayCreation()
      
    },
  }
</script>

<style lang="scss" scoped>
.my-div{
  width: 55%;
  background-color: #313131;
  // border-radius: 1.5em;
  margin: 40px 0;
  padding: 20px;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
}
.disk{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    // border: 5px solid grey;
    // border: 2px solid #ba8a2a;
    background-color: #0a0b0c;
    margin: 5px;
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
  background-color: #313131;
  margin: 40px 0;
  padding: 20px;
  -webkit-box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0); 
  box-shadow: 0px 10px 13px -7px #000000, 5px 5px 15px 5px rgba(0,0,0,0);
}
.disabled{
  pointer-events: none;
}
.win{
  position: absolute;
  top: 35%;
  left: 40%;
  background-color: #313131;
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

</style>
