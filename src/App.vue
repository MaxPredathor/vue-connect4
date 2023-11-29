<template>
  <div>

  </div>
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

</style>
