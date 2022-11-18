<template>
  <div class="home">
    <h1>DropZone</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile"/>
    <span class="file-info">File: {{dropZoneFile.name}}</span>
  </div>
</template>

<script>
// @ is an alias to /src
import DropZone from '@/components/DropZone.vue'
import {ref} from "vue"
import {BlobServiceClient} from "@azure/storage-blob"

export default {
  name: 'HomeView',
  components: {
    DropZone
},
setup(){
  let dropZoneFile = ref("");

  const connectionString=" ";
  const containerName = "images";
  const blobServiceClient = BlobServiceClient.fromConnectionString(connectionString);
  const containerClient = blobServiceClient.getContainerClient(containerName);

  const send = async (file) =>{
    const blockBlobClient = containerClient.getBlockBlobClient(file.name);
    await blockBlobClient.upload(file.content, file.length);
  };
  
  const drop = async (e) =>{
    const file = e.dataTransfer.files[0];
    dropZoneFile.value = file;
    await send(file);
  };

  const selectedFile = async () =>{
    const file = document.querySelector('.dropZoneFile').files[0];
    dropZoneFile.value = file;
    await send(file);
  };

  return {dropZoneFile, drop, selectedFile};
},
};
</script>
<style lang="scss" scoped>
.home{
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f1f1f1;
  .file-info{
    margin-top: 20px;
  }
  h1 { 
    font-size: large;
    margin-bottom: 32px;
  }
}
</style>
