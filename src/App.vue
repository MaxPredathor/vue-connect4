<template>
  <main>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 my-div d-flex justify-content-center">
          <div v-for="(rows, rowsIndex) in biArray" :key="rowsIndex">
            <div class="disk" v-for="(cols, colsIndex) in rows" :key="colsIndex" @click="insertDisk(rowsIndex, colsIndex)"
             :class="{'disk-blue': cols === 1, 'disk-red': cols === 0 }" ></div>
          </div>
        </div>
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
  </main>
</template>

<script>
import { store } from './assets/data/store'
  export default {
    name: 'App',
    data(){
      return{
        store,
        rows: 8,
        cols: 6,
        biArray: [],
        redBlueSwitch: true,
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
          }else{
            this.diskVerticalCheck(x, 0)
          }
        }    
      },
      // gameWin(){
      //   if(){
          
      //   }
      // }
    },
    created(){
      this.arrayCreation()
    },
  }
</script>

<style lang="scss" scoped>
.my-div{
  width: 70%;
  background-color: #a6a6a6;
  border-radius: 2em;
  margin: 40px 0;
  padding: 20px;

  
}
.disk{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    border: 5px solid grey;
    background-color: lightgray;
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
      color: rgb(14, 14, 140);
      font-weight: bold;
      font-size: 30px;
    }
  }
  .red{

    p{
      color: rgb(175, 24, 24);
      font-weight: bold;
      font-size: 30px;
    }
  }

</style>
