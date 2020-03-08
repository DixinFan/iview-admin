<template>
    <div id="app">
        <Input v-model="value" placeholder="输入视频标题" style="margin-left:30px;width: 300px" />
        <a href="javascript:;" class="file">选择文件
            <input type="file" @change="onFileChange" name="" id="">
        </a>
        <Button style="margin-left:10px">上传</Button>
        <nobr style="margin-left:30px"></nobr>
        <nobr v-for="item in file" :key='item.keyID' style="margin-left:10px">{{ item.name }}
            <a href="javascript:;"  @click="delectFile(item.keyID)">X</a>
        </nobr>
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
      url: 'http://localhost:8081/uploads/no-preview-available.png',
      value: '',
      file: [],
      uploadFile: [] // 需要上传的文件List
    }
  },
  methods: {
    onFileChange (e) {
      const file = e.target.files[0]
      console.log(file)
      console.log(file.type)
      if (file.type === 'image/jpeg') {
        console.log('how you doing')
      }
      this.url = URL.createObjectURL(file)
      let keyID = Math.random().toString().substr(2)
      file['keyID'] = keyID
      this.file.push(file)
      this.uploadFile.push(file)
    },
    handleUpload (file) { // 保存需要上传的文件
      let keyID = Math.random().toString().substr(2)
      file['keyID'] = keyID
      this.file.push(file)
      this.uploadFile.push(file)
      return false
    },
    delectFile (keyID) { // 删除文件
      this.file = this.file.filter(item => {
        return item.keyID !== keyID
      })
      this.uploadFile = this.uploadFile.filter(item => {
        return item.keyID !== keyID
      })
    },
    upload () { // 上传文件
      if (this.uploadFile.length === 0) {
        this.$Message.error('未选择上传文件')
        return false
      }
      for (let i = 0; i < this.uploadFile.length; i++) {
        let item = this.file[i]
        this.$refs.upload.post(item)
      }
    },
    uploadSuccess (response, file, fileList) { // 文件上传回调 上传成功后删除待上传文件
      console.log(response) // 后端返回数据
      console.log(file) // 当前上传文件
      console.log(fileList) // 整个input file 里的文件数组
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
    .file {
        margin: -11px 30px;
        position: relative;
        display: inline-block;
        background: #D0EEFF;
        border: 1px solid #99D3F5;
        border-radius: 4px;
        padding: 4px 12px;
        overflow: hidden;
        color: #1E88C7;
        text-decoration: none;
        text-indent: 0;
        line-height: 20px;
    }
    .file input {
        position: absolute;
        font-size: 100px;
        right: 0;
        top: 0;
        opacity: 0;
    }
    .file:hover {
        background: #AADFFD;
        border-color: #78C3F3;
        color: #004974;
        text-decoration: none;
    }
</style>
