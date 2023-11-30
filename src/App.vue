<template>
  <main>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 my-div d-flex justify-content-center">
          <div v-for="(rows, rowsIndex) in biArray" :key="rowsIndex">
            <div class="disc" v-for="(cols, colsIndex) in rows" :key="colsIndex" @click="redBlueSwitch = !redBlueSwitch, insertDisc(rowsIndex, colsIndex)"
             :class="{'disc-blue': cols === 1, 'disc-red': cols === 0 }" ></div>
          </div>
        </div>
        <div v-if="redBlueSwitch" class="d-flex justify-content-center align-items-center blue">
          <p>Blue Turn </p>
          <div class="disc-blue"></div>
        </div>
        <div class="d-flex justify-content-center align-items-center red" v-else>
          <p>Red Turn </p>
          <div class="disc-red"></div>
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
        redBlueSwitch: true
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
        this.biArray[0][5] = 1
        console.log(this.biArray)
      },
      insertDisc(x, y){
        // if(this.biArray[disc][item] !== 1 || this.biArray[disc][item] !== 0){
          if(this.redBlueSwitch){
          this.biArray[x][y] = 1
          } else {
          this.biArray[x][y] = 0
          }  
        // }   
      }
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
.disc{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    border: 5px solid grey;
    background-color: lightgray;
    margin: 5px;
  }
  .disc-blue{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    border: 5px solid rgb(5, 5, 68);
    background-color: rgb(14, 14, 140);
    margin: 5px;
  }
  .disc-red{
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
