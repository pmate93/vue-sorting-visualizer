<template>
  <Header @generate-array="generateArray" @sort="sort" :disabled="disabled" />
  <NumsArray 
  :numsArray="numsArray" 
  :value_i="value_i" 
  :value_j="value_j" 
  :value_largest="value_largest"
  :pivot="pivot" 
  :turnRed="turnRed" 
  :counter="counter" 
  :sortType="sortType"
  :finalArray="finalArray"
  :done="done"
  :doneGreen="doneGreen"
  :disabled="disabled" />
</template>

<script>
import Header from './components/Header.vue';
import NumsArray from './components/NumsArray.vue';

export default {
  name: 'App',
  components: {
    Header,
    NumsArray
  },

  data(){
    return{
      numsArray: [],
      value_i: 0,
      value_j: 0,
      value_largest: 0,
      turnRed: false,
      pivot: 0,
      counter: -1,
      doneGreen: false,
      done: false,
      disabled: false,
      sortType: '',
      valuesToSwap: [
        {first: 0},
        {second: 0},
        {swap: false}
      ],
      valuesToSwapQuick:[
        {i: 0},
        {j: 0},
        {pivot: -1},
        {swap: false}
      ],
      valuesToSwapHeap: [
        {i: 0},
        {j: 0},
        {largest: 0},
        {swap: false}
      ],
      finalArray: [],
    }
  },

  methods: {
    generateArray(range){
      this.done = false;
      this.numsArray = [];
      this.counter = -1;
      this.value_i = -1;
      this.value_j = -1;
      this.value_largest = -1;
      
      while(this.numsArray.length < range){
        let duplicate = false;
        let num = Math.floor(Math.random() * 100) + 1;
        for(let i = 0; i < this.numsArray.length;i++){
          if(this.numsArray[i]===num){
            duplicate = true;
            break;
          }
        }

        if(!duplicate){
          this.numsArray.push(num);
        }
      }
    },

    async sort(type){
      this.sortType = type;
      this.done = false;
      this.disabled = true;

      const sleep = (time) => {
        return new Promise((resolve) => setTimeout(resolve, time))
      }

      let time = (180 / this.numsArray.length) * 10;
      this.counter = this.numsArray.length;

      if(type === 'Bubble Sort'){

        for(let i = 0; i < this.numsArray.length; i++){
          for(let j = 0; j < (this.numsArray.length - i - 1); j++){
            await sleep(time);
            this.value_j = j;
            this.value_i = j + 1;
            if(this.numsArray[j] > this.numsArray[j+1]){
              await sleep(time);
              this.turnRed = true;
              await sleep(time);
              let temp = this.numsArray[j];
              this.numsArray[j] = this.numsArray[j + 1];
              this.numsArray[j + 1] = temp;
              await sleep(time);
              this.turnRed = false;
            } 
          }
          this.counter--;
        }
        setTimeout(() =>{
              this.doneGreen = true;
              setTimeout(()=>{
                this.doneGreen = false;
                this.done = true;
                this.disabled = false;
              }, 500)
            },50);
      }else if(type ==='Merge Sort'){

        let tomb = [...this.numsArray];
        let previousArrays = [];
        this.valuesToSwap = [];
        this.mergeSort(tomb, 0, tomb.length-1, previousArrays);
        previousArrays.reverse();
        this.valuesToSwap.reverse();

        const printDelayed = () =>{
          let time = (80 / tomb.length) * 10;
          this.turnRed = false;
          if(previousArrays.length != 0){
            let pair = this.valuesToSwap.pop();

            if(pair.first !== pair.second){
              this.value_i = pair.first;
              this.value_j = pair.second;

            }else{
              time = 0;
            }

            setTimeout(() =>{
              if(pair.swap === true){
                this.turnRed = true;
              }
              setTimeout(() =>{
                this.numsArray = previousArrays.pop();
                if(pair.swap === true){
                  this.turnRed = true;
                  this.numsArray = previousArrays[previousArrays.length -1];
                  
                  setTimeout(() =>{
                    setTimeout(printDelayed, time);
                  }, time)
                }else{

                  setTimeout(printDelayed, time);
                }
                
                
              },time)
          },time)
          }else{
            setTimeout(() =>{
              this.doneGreen = true;
              setTimeout(()=>{
                this.doneGreen = false;
                this.done = true;
                this.disabled = false;
              }, 500)
            },50);
          }
          
        }
        printDelayed();
      }else if(type ==='Quick Sort'){
        this.counter = -1;
        this.valuesToSwapQuick = [];


        let tempArray = [...this.numsArray];
        this.finalArray = this.quickSort(tempArray, 0, tempArray.length-1);
        this.valuesToSwapQuick.reverse();
        this.finalArrayToCheck = [];
        this.pivot = this.valuesToSwapQuick[this.valuesToSwapQuick.length-1].pivot;

        const printDelayed = () =>{
          this.turnRed = false;
          if(this.valuesToSwapQuick.length!=0){
            let pair = this.valuesToSwapQuick.pop();
            this.value_i = pair.i;
            this.value_j = pair.j;

            if(this.pivot !== pair.pivot){
              this.counter++;
            }
            
            this.pivot = pair.pivot;

            if(pair.swap && (pair.i !== pair.j)){
              setTimeout(()=>{
                this.turnRed = true;
                setTimeout(()=>{
                  let temp = this.numsArray[pair.i];
                  this.numsArray[pair.i] = this.numsArray[pair.j];
                  this.numsArray[pair.j] = temp;
                  setTimeout(()=>{
                    this.turnRed = false;
                    setTimeout(printDelayed, time);
                  }, time);
                },time)
              },time)
            }else{
              setTimeout(printDelayed, time);
            }
          }else{
            setTimeout(() =>{
              this.pivot = -1;
              this.doneGreen = true;
              setTimeout(()=>{
                this.doneGreen = false;
                this.done = true;
                this.disabled = false;
              }, 500)
            },50);
          
          }
        }
        printDelayed();


      }else if(type ==='Heap Sort'){
        this.valuesToSwapHeap = [];
        let tempArray = [...this.numsArray];

        this.heapSort(tempArray);
        this.valuesToSwapHeap.reverse();

        const printDelayed = () =>{
          this.turnRed = false;
          if(this.valuesToSwapHeap.length!=0){
            let pair = this.valuesToSwapHeap.pop();
            this.value_i = pair.i;
            this.value_j = pair.j;
            this.value_largest = pair.largest;

            let temp;
            if(pair.swap && pair.largest !== -1){
              setTimeout(()=>{
                this.turnRed = true;
                setTimeout(()=>{
                  temp = this.numsArray[pair.i];
                  this.numsArray[pair.i] = this.numsArray[pair.largest];
                  this.numsArray[pair.largest] = temp;
                  setTimeout(()=>{
                    this.turnRed = false;
                    setTimeout(printDelayed, time);
                  }, time);
                }, time)
              }, time)

            }else if(pair.swap && pair.largest === -1){
              setTimeout(()=>{
                this.turnRed = true;
                setTimeout(()=>{
                  temp = this.numsArray[pair.i];
                  this.numsArray[pair.i] = this.numsArray[pair.j];
                  this.numsArray[pair.j] = temp;
                  setTimeout(()=>{
                    this.turnRed = false;
                    this.counter--;
                    setTimeout(printDelayed, time);
                  }, time);
                }, time)
              }, time)
            }else{
              setTimeout(printDelayed, time);
            }
          }else{
            setTimeout(() =>{
              this.doneGreen = true;
              setTimeout(()=>{
                this.doneGreen = false;
                this.done = true;
                this.disabled = false;
              }, 500)
            },50);
          }
        }

        printDelayed();
      }
    },

    heapSort(arr){
      let n = arr.length;

      // Build heap (rearrange array)
      for (let i = Math.floor(n / 2) - 1; i >= 0; i--){
        this.heapify(arr, n, i);
      }

      // One by one extract an element from heap
      for (let i = n - 1; i > 0; i--) {
        this.valuesToSwapHeap.push({
            i: i,
            j: 0,
            largest: -1,
            swap: true
          })
        // Move current root to end
        let temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;

        // call max heapify on the reduced heap
        this.heapify(arr, i, 0);
      }
    },
 
    // To heapify a subtree rooted with node i which is
    // an index in arr[]. n is size of heap
    heapify(arr, n, i){
      let largest = i; // Initialize largest as root
      let l = 2 * i + 1; // left = 2*i + 1
      let r = 2 * i + 2; // right = 2*i + 2

      

      // If left child is larger than root
      if (l < n && arr[l] > arr[largest]){
        largest = l;
        
      }

      // If right child is larger than largest so far
      if (r < n && arr[r] > arr[largest]){
        largest = r;
        
      }

      // If largest is not root
      if (largest != i) {
        if(largest === r){
          this.valuesToSwapHeap.push({
            i: i,
            j: l,
            largest: largest,
            swap: true
          })
        }else{
          this.valuesToSwapHeap.push({
            i: i,
            j: r,
            largest: largest,
            swap: true
          })
        }
        let swap = arr[i];
        arr[i] = arr[largest];
        arr[largest] = swap;

        // Recursively heapify the affected sub-tree
        this.heapify(arr, n, largest);

      }else if(l <= n && r <= n){
        this.valuesToSwapHeap.push({
          i: i,
          j: l,
          largest: r,
          swap: false
        })
      }
    },

    quickSort(arr, left, right) {
      let i;
      if (arr.length > 1) {
          i = this.partition(arr, left, right);
          if (left < i - 1) {
            this.quickSort(arr, left, i - 1);
          }
          if (i < right) {
            this.quickSort(arr, i, right);
          }
      }
      return arr;
    },

    partition(arr, left, right){
      let pivot = arr[Math.floor((left+right)/2)];

      let i = left; //left pointer
      let j = right; //right pointer (last element of array)
      
      while(i <= j){
        while(arr[i]<pivot){
          this.valuesToSwapQuick.push({
            i: i,
            j: j,
            pivot: Math.floor((left+right)/2),
            swap: false
          })
          i++;
        }

        while(arr[j]>pivot){
          this.valuesToSwapQuick.push({
            i: i,
            j: j,
            pivot: Math.floor((left+right)/2),
            swap: false
          })
          j--;
        }

        if(i <= j){
          this.valuesToSwapQuick.push({
            i: i,
            j: j,
            pivot: Math.floor((left+right)/2),
            swap: true
          })
          let temp = arr[i];
          arr[i] = arr[j];
          arr[j] = temp;
          i++;
          j--;
        }
      }
      
      return i;
    },

    merge(arr, l, m, r, previousArrays) {
      let n1 = m - l + 1;
      let n2 = r - m;

      // Create temp arrays
      let L = new Array(n1);
      let R = new Array(n2);

      for (let i = 0; i < n1; i++){ 
        L[i] = arr[l + i];
      }
      for (let j = 0; j < n2; j++){
        R[j] = arr[m + 1 + j];
      };

      //index of first subarray
      let i = 0;

      //index of second subarray
      let j = 0;

      //index of merged subarray
      let k = l;

      while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
          this.valuesToSwap.push({
            first: L[i],
            second: R[j],
            swap: false
          })
          arr[k] = L[i];
          i++;
        } else {
          this.valuesToSwap.push({
            first: R[j],
            second: L[i],
            swap: true
          })
          arr[k] = R[j];
          j++;
        }
        k++;
        previousArrays.push([...arr]);
      }

      while (i < n1) {
        this.valuesToSwap.push({
          first: L[i],
            second: arr[k],
            swap: false
          })
        arr[k] = L[i];
        i++;
        k++;
        previousArrays.push([...arr]);
      
      }

      while (j < n2) {
        this.valuesToSwap.push({
            first: R[j],
            second: arr[k],
            swap: false
          })
        arr[k] = R[j];
        j++;
        k++;
        previousArrays.push([...arr]);
      }
    },
    
    mergeSort(arr, startIdx, endIdx, previousArrays) {
      if (startIdx >= endIdx) {
        return;
      }
      let midIdx = startIdx + parseInt((endIdx - startIdx) / 2);
      this.mergeSort(arr, startIdx, midIdx, previousArrays);
      this.mergeSort(arr, midIdx + 1, endIdx, previousArrays);
      this.merge(arr, startIdx, midIdx, endIdx, previousArrays);
    },

  },


  created(){
    this.generateArray(50);
  }
  
}
</script>

<style>


body{
  margin: 0px;
  background-color: rgb(48, 46, 46);
}
</style>
