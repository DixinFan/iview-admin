<template>
    <Upload
        multiple
        type="drag"
        name="video"
        action="//localhost:8081/index.php/index/index/upload/">
        <div style="padding: 20px 0">
            <Icon type="ios-cloud-upload" size="52" style="color: #3399ff"></Icon>
            <p>Click or drag files here to upload</p>
        </div>
    </Upload>
</template>
<script>
export default {

}
</script>
