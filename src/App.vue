<template>
  <main>
    <div class="container">
      <div class="row">
        <div class="col-12 my-div d-flex flex-nowrap">
          <div class="disc" v-for="disc in myArray">
            <div class="disc" v-for="item in myArray[0]"></div>
          </div>
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
        rows: 10,
        cols: 8,
        myArray: [],
      }
    },
    methods:{
      arrayCreation(){
        for(let i = 0; i < this.rows; i++){
          this.myArray[i] = []
          for(let j = 0; j < this.cols; j++){
            this.myArray[i][j] = null
          }
        }
        console.log(this.myArray)
      }
    },
    created(){
      this.arrayCreation()
    },
  }
</script>

<style lang="scss" scoped>
.my-div{
  width: 100%;
  background-color: #a6a6a6;
  border-radius: 2em;
  margin: 40px 0;
  padding: 20px;

  .disc{
    width: 75px;
    height: 75px;
    border-radius: 50%;
    border: 5px solid grey;
    background-color: lightgray;
    margin: 5px;
  }
}

</style>
