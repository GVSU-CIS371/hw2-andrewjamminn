<template>
  <div>
    <Beverage
      :isIced="store.currentTemp === 'Cold'"
      :baseClass="store.currentBase?.id"
      :syrupClass="store.currentSyrup?.id"
      :creamClass="store.currentCreamer?.id"
    />
    <ul>
      <li>
        <!--buttons for temps-->
        <template v-for="temp in store.temps" :key="temp">
          <label>
            <input
              type="radio"
              name="temperature"
              :id="`r${temp}`"
              :value="temp"
              v-model="store.currentTemp"
            />
            {{ temp }}
          </label>
        </template>
      </li>
      <li>
        <!--buttons for bases-->
        <template v-for="base in store.bases" :key="base">
          <label>
            <input
              type="radio"
              name="base"
              :id="`r${base}`"
              :value="base"
              v-model="store.currentBase"
            />
          </label>
          {{ base.name }}
        </template>
      </li>
      <!--buttons for creamer-->
      <li>
        <template v-for="cream in store.creamers" :key="cream">
          <label>
            <input
              type="radio"
              name="cream"
              :id="`r${cream}`"
              :value="cream"
              v-model="store.currentCreamer"
            />
          </label>
          {{ cream.name }}
        </template>
      </li>
      <!--buttons for syrup-->
      <li>
        <template v-for="syrup in store.syrups" :key="syrup">
          <label>
            <input
              type="radio"
              name="syrup"
              :id="`r${syrup}`"
              :value="syrup"
              v-model="store.currentSyrup"
            />
          </label>
          {{ syrup.name }}
        </template>
      </li>
      <!-- Name input and Make Beverage button -->
      <li class="recipe-create">
        <label>
          Name:
          <input type="text" v-model="beverageName" placeholder="Beverage Name" />
        </label>
        <button @click="createBeverage">Make Beverage</button>
      </li>
    </ul>
    
    <!-- Display saved beverages as radio buttons -->
    <div v-if="store.savedBeverages.length > 0" class="saved-beverages">
      <h3>Saved Beverages</h3>
      <ul class="beverage-list">
        <li v-for="(beverage, index) in store.savedBeverages" :key="index">
          <label>
            <input 
              type="radio" 
              name="savedBeverages"
              :value="index"
              v-model="selectedBeverageIndex"
              @change="loadSelectedBeverage"
            />
            {{ beverage.name }} - {{ beverage.temperature }} {{ beverage.base.name }} with 
            {{ beverage.creamer.name }} and {{ beverage.syrup.name }}
          </label>
        </li>
      </ul>
    </div>
  </div>
  
</template>
<script setup lang="ts">
import Beverage from "./components/Beverage.vue";
import { useBeverageStore } from "./stores/beverageStore";
import { ref } from 'vue';

const store = useBeverageStore();
const beverageName = ref('');
const selectedBeverageIndex = ref<number | null>(null);

const createBeverage = () => {
  store.makeBeverage(beverageName.value || 'Untitled Beverage');
  beverageName.value = ''; // Clear the input after creating
};

const loadSelectedBeverage = () => {
    store.showBeverage(selectedBeverageIndex.value || 0 );
};

</script>
<style lang="scss">
body,
html {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  background-color: #6e4228;
  background: linear-gradient(to bottom, #6e4228 0%, #956f5a 100%);
}
ul {
  list-style: none;
}
.recipe-create {
  margin-top: 15px;
  padding: 10px;
  
  label {
    margin-right: 10px;
  }
  
  input[type="text"] {
    padding: 5px;
    margin-right: 10px;
  }
  
  button {
    padding: 5px 10px;
    background-color: #4a2c17;
    color: white;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    
    &:hover {
      background-color: #6e4228;
    }
  }
}

.saved-beverages {
  margin-top: 20px;
  padding: 15px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 5px;
  
  h3 {
    margin-top: 0;
  }
  
  ul {
    padding-left: 10px;
  }
  
  li {
    margin-bottom: 5px;
  }
}
</style>