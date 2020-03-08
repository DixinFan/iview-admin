<template>
    <!-- <div id="upload" style="margin:50px 200px;"> -->
    <div id="upload">
        <Upload
        multiple
        ref="upload"
        :before-upload="handleUpload"
        :show-upload-list="false"
        :on-success="uploadSuccess"
        action="//jsonplaceholder.typicode.com/posts/">
        <Button type="ghost" icon="ios-cloud-upload-outline">浏览</Button>
        </Upload>
        <div v-for="(item, index) in file" :key='item'>Upload file: {{ item.name }}
            <a href="javascript:;"  @click="delectFile(item.keyID)">X</a>
            <Button style="margin-left:30px;" size="small" v-if="index === 0" type="primary" @click="upload" >上传</Button>
        </div>
    </div>
</template>

<script src="https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js"></script>
<script>
import Cookies from 'js-cookie'
export default {
    data() {
        return{
            file: [
                {
                    name: 'aaa.jpg',
                    type: 'image/jpeg',
                    keyID: 123479
                },
                {
                    name: 'bbb.jpg',
                    type: 'image/jpeg',
                    keyID: 987654
                }
            ], // 总文件List
            uploadFile: [], // 需要上传的文件List
        }
    },
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
        },
        handleUpload (file) { // 保存需要上传的文件
            let keyID = Math.random().toString().substr(2);
            file['keyID'] = keyID;
            this.file.push(file);
            this.uploadFile.push(file)
            return false;
        },
        delectFile (keyID) { // 删除文件
            this.file = this.file.filter(item => {
                return item.keyID != keyID
            })
            this.uploadFile = this.uploadFile.filter(item => {
                return item.keyID != keyID
            })
        },
        upload () { // 上传文件
            if(this.uploadFile.length === 0 ) {
                this.$Message.error('未选择上传文件')
                return false
            }
            for (let i = 0; i < this.uploadFile.length; i++) {
                let item = this.file[i]
                this.$refs.upload.post(item);
            }
        },
        uploadSuccess (response, file, fileList) { // 文件上传回调 上传成功后删除待上传文件
            console.log(response) // 后端返回数据
            console.log(file)   // 当前上传文件
            console.log(fileList) // 整个input file 里的文件数组
        },
    }
}
</script>

<style>

</style>
