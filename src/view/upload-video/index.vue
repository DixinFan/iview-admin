<template>
    <Upload
        multiple
        type="drag"
        name="video"
        :before-upload="beforeUpload"
        action="//localhost:8081/index.php/index/index/upload/">
        <div style="padding: 20px 0">
            <Icon type="ios-cloud-upload" size="52" style="color: #3399ff"></Icon>
            <p>Click or drag files here to upload</p>
        </div>
    </Upload>
</template>

<script src="https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js"></script>
<script>
import Cookies from 'js-cookie'
export default {
  methods: {
    beforeUpload(file) {
        this.handleSubmit(file);
        return false;    // 阻止使用iview的上传功能
    },
    async handleSubmit(file){
        let param = new FormData();
        param.append('video',file);
        var userName = Cookies.get('userName');
        param.append('userName',userName);
        console.log(file);
        console.log(userName);
        let config = {
          headers:{'Content-Type':'multipart/form-data'}
        };
        console.log('Still good');
        this.$http.post('http://localhost:8081/index.php/index/index/uploadVideo/',param,config)
          .then(response=>{
            console.log(response.data);
          })
          .catch(function (error) {console.log(error);});
    }
  }
}
</script>

<style>

</style>
