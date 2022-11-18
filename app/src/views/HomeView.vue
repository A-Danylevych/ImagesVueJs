<template>
  <div class="home">
    <h1>DropZone</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile"/>
    <span class="file-info">File: {{dropZoneFile.name}}</span>
  </div>
</template>

<script>
import DropZone from '@/components/DropZone.vue'
import {ref} from "vue"
import {BlobServiceClient} from "@azure/storage-blob"

export default {
  name: 'HomeView',

  components: {
    DropZone
  },

  data(){
    const connectionString=" ";
    const containerName = "images";
    const blobServiceClient = BlobServiceClient.fromConnectionString(connectionString);
    const containerClient = blobServiceClient.getContainerClient(containerName);
    return {containerClient};
  },

  setup(){
    let dropZoneFile = ref(""); 

    const drop = async (e) =>{
    const file = e.dataTransfer.files[0];
    dropZoneFile.value = file;
    await this.send(file);
    };

    const selectedFile = async () =>{
    const file = document.querySelector('.dropZoneFile').files[0];
    dropZoneFile.value = file;
    await this.send(file);
    };
    
    return {dropZoneFile, drop, selectedFile};
  },

  methods: {
    async send(file){
      const blockBlobClient = this.containerClient.getBlockBlobClient(file.name);
      await blockBlobClient.upload(file.content, file.length);
    },
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
