<template>
<div @dragenter.prevent="toggleActive" @dragleave.prevent="toggleActive"
@dragover.prevent @drop.prevent="toggleActive" :class="{'activeDropZone':active}" class="dropZone">
  <span>Drage or Drop File</span>
  <span>Or</span>
  <label for="dropZoneFile">Select File</label>
  <input type="file" id="dropZoneFile" ref="image"
        accept="image/*" class="dropZoneFile"/>
</div>
</template>

<script>
import {ref} from "vue"
export default {
  name: 'DropZone',
  
  setup(){
    const active = ref(false);

    const toggleActive = () =>{
      active.value = !active.value
    };
    
    return {active, toggleActive}
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.dropZone{
  width: 400px;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-self: center;
  align-items: center;
  row-gap: 16px;
  border: 2px dashed #41b883;
  background-color: #fff;
  transition: .3s ease all;

  label{
    padding: 8px 12px;
    color: #fff;
    background-color: #41b883;
    transform: .3s ease all;
  }
  input{
    display: none;
  }
}
.activeDropZone{
    color: #fff;
    border-color: #fff;
    background-color: #41b883;
    label{
      background-color: #fff;
      color: #41b883;
    }
  }
</style>
