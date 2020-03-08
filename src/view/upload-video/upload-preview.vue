<template>
    <div id="app">
        <Input v-model="value" placeholder="输入视频标题" style="width: 300px" />
        <input type="file" @change="onFileChange" />
        <br/>
        <br/>
        <div id="preview">
            <img v-if="url" :src="url" alt="http://localhost:8081/uploads/no-preview-available.png"/>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      // url: null,
      url: 'http://localhost:8081/uploads/no-preview-available.png'
    }
  },
  methods: {
    onFileChange (e) {
      const file = e.target.files[0]
      this.url = URL.createObjectURL(file)
    }
  }
}
</script>

<style scoped>
    body {
    background-color: #e2e2e2;
    }

    #app {
    padding: 20px;
    }

    #preview {
    display: flex;
    justify-content: center;
    align-items: center;
    }

    #preview img {
    max-width: 100%;
    max-height: 500px;
    }
</style>
