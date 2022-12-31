<script setup lang="ts">

import { ref, onMounted ,  watch } from 'vue'

    const arrayValue = ref(10);
    const speedValue = ref(1); 
    const selectedSort = ref("No Sort Selected");

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

    async function sort() {
        if (selectedSort.value === "Insertion Sort") {
            await insertionSort();
        } else if (selectedSort.value === "Bubble Sort") {
            await bubbleSort();
        }
    }


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

    async function sleep() {
      return new Promise((resolve) => setTimeout(resolve, speedValue.value));
    }

    onMounted(() => {
        columns = Array.from({length: arrayValue.value}, () => Math.floor(Math.random() * 100));
        console.log(columns);
        updateDOM.value+=1;
    })

    
</script>

<template>
    
    <div class="mainDiv has-text-centered">

        <h1 class="title textWhiteBold">Vue Sorting Visualizer</h1>     
        <br>

        <p class="textWhite"> Selected an algorithm with the dropdown and press the sort button to watch the list get sorted!</p>

        <br>
        
        <div class="container slider">
            <o-field  class="" label="Size Of Array" >
                <o-slider v-model="arrayValue" :tooltip-variant="sliderType" variant="danger" :min="5" max="50"></o-slider>
            </o-field>
        </div>
        
        <div class="container slider">
            <o-field label="Speed Of Sort" >
                <o-slider v-model="speedValue" :tooltip-variant="sliderType" variant="danger" :min="1" max="50"></o-slider>
            </o-field>
        </div>

        <br>
        <br>

        <div class="container">
            <o-button @click="sort()" variant="warning">Sort</o-button>
        </div> 

        <br>
        
        <div class="container">
            <o-dropdown :triggers="['hover']" v-model="selectedSort" aria-role="list">
                <template #trigger="{ active }">
                    <o-button variant="warning">
                    <span>{{ selectedSort }}</span>
                    </o-button>
                </template>
                <div>
                    <o-dropdown-item aria-role="listitem" value="Insertion Sort">Insertion Sort</o-dropdown-item>
                    <o-dropdown-item aria-role="listitem" value="Insertion Sort">Bubble Sort</o-dropdown-item>
                </div>
            </o-dropdown>
        </div> 
        <!-- <p class="textWhiteBold">Algorithm Selected: {{ selectedSort }}</p>       -->
        <br>
        <br>

        <div class="container cols box" :key="updateDOM">
            <div class="bar" v-for="number,index in columns" :key="index" :style="{height: (number *3) + 'px', width: (70/arrayValue) + '%'}"></div>
        </div>
    </div>
</template>

<style scoped>
/* @import "https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css"; */

.mainDiv {
    padding-left: 7.5%;
    padding-right: 7.5%;
    background-color: #f95959;
    padding-top:5%;
    padding-bottom:50%;

}

.textWhiteBold{
    color: white;
    /* make font bold */
    font-weight: bold;
}
.textWhite {
    color: white;
    font-size:large;
}

.cols {
  width: 75%;
  background-color: #f95959;

}
.slider {
    width: 25%;
}
.bar {
  width: 20px;
  background-color: #455d7a;
  display: inline-block;
   margin: 0 2px;
}
body {
}
</style>