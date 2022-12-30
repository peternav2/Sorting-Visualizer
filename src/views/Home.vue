<script setup lang="ts">

import { ref, reactive,  watch } from 'vue'

    const arrayValue = ref(0);
    const speedValue = ref(5);

    const sliderType = 'warning'; 
    let columns: number[] = [];
    const updateDOM = ref(0);

    watch(speedValue, () => {
        if (speedValue.value === 0) {
            speedValue.value = 1;
        }
    })
    watch(arrayValue, () => {
        columns = Array.from({length: arrayValue.value}, () => Math.floor(Math.random() * 100));
        console.log(columns);
        updateDOM.value+=1;
    })



     async function bubbleSort(){
      let checked = false;
      do {
        checked = false;
        for(let i =0;i<arrayValue.value;i++){
          if(columns[i] > columns[i+1]){
            let temp = columns[i];
            columns[i] = columns[i+1];
            columns[i+1] = temp;
            // sleep - to visualize / see the changes
            await sleep();
            checked = true;
            updateDOM.value+=1;
          }
        }
      } while(checked)
    }

    async function insertionSort() {
        let i = 0;
        let key = 0;
        let j = 0;

        for (i = 1; i < arrayValue.value; i++) {
            key = columns[i];
            j = i-1;
            while (j >= 0 && columns[j] > key) {
                columns[j+1] = columns[j];
                j=j-1;
                await sleep();
                updateDOM.value+=1;
            }
            columns[j+1] = key;
        }
    }

    function sleep() {
      return new Promise((resolve) => setTimeout(resolve,  (1000/((speedValue.value*speedValue.value)))));
    }
    
</script>

<template>
    
    <div class="mainDiv has-text-centered">

        <h1 class="title">Vue Sorting Visualizer</h1>     
        <br>
        <div class="container cols">
            <o-field label="Size Of Array" >
                <o-slider v-model="arrayValue" :tooltip-variant="sliderType" variant="danger" max="200"></o-slider>
            </o-field>
        </div>
        <br>
        <div class="container cols">
            <o-field label="Speed Of Sort" >
                <o-slider v-model="speedValue" :tooltip-variant="sliderType" variant="danger" max="200"></o-slider>
            </o-field>
        </div>
        <div class="container">
            <o-button @click="bubbleSort()" variant="danger">Bubble Sort</o-button>
            <o-button @click="insertionSort()" variant="danger">Insertion Sort</o-button>
        </div>        
        <br>


        <div class="container cols" :key="updateDOM">
            <div class="bar" v-for="number,index in columns" :key="index" :style="{height: number + 'px', width: (100/arrayValue) + '%'}"></div>
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