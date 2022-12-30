<script setup lang="ts">

import { ref, reactive,  watch } from 'vue'

    const sliderValue = ref(5)
    const sliderType = 'warning' 
    let columns: number[] = [];
    const value = ref(0);
    watch(sliderValue, () => {
        columns = Array.from({length: sliderValue.value}, () => Math.floor(Math.random() * 100));
        console.log(columns)
        value.value+=1;
    })

     async function bubbleSort(){
      let checked = false;
      do {
        checked = false;
        for(let i =0;i<sliderValue.value;i++){
          if(columns[i] > columns[i+1]){
            let temp = columns[i];
            columns[i] = columns[i+1];
            columns[i+1] = temp;
            // sleep - to visualize / see the changes
            await sleep()
            checked = true
            value.value+=1;
          }
        }
      } while(checked)
    }
    function sleep() {
      return new Promise((resolve) => setTimeout(resolve, 50));
    }
    
</script>

<template>
    
    <div class="mainDiv has-text-centered">

        <h1 class="title">Vue Sorting Visualizer</h1>     
        <br>
        <div class="container cols">
            <o-field label="Size Of Array" >
                <o-slider v-model="sliderValue" :tooltip-variant="sliderType" variant="danger"></o-slider>
            </o-field>
        </div>
        
        <div class="container">
            <o-button @click="bubbleSort()" variant="danger">Bubble Sort</o-button>
        </div>        
        <br>


        <div class="container cols" :key="value">
            <div class="bar" v-for="number,index in columns" :key="index" :style="{height: number + 'px', width: (100/sliderValue) + '%'}"></div>
        </div>
    </div>
</template>

<style scoped>
/* @import "https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css"; */

.mainDiv {
    padding-left: 7.5%;
    padding-right: 7.5%;
}
.cols {
  width: 50%;
}
.slider {
    width: 25%;
}
.bar {
  width: 20px;
  background-color: black;
  display: inline-block;
  /* margin: 0 2px; */
}
</style>