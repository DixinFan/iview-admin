<template>
  <div class="hello">
    <div v-for="(item,i) in list" :key="i" style="margin-bottom: 30px">
      <video :id="'myVideo'+item.id" class="video-js">
        <source :src="item.src" type="video/mp4">
      </video>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Cookies from 'js-cookie'
var count = -1
var video_list = []
export default {
  name: 'HelloWorld',
  data () {
    return {
      list: [
        {
          src: 'http://img.yopoo.cn/banner_video.mp4 ',
          id: 0,
          pic: ''
        },
        {
          src: 'http://img.yopoo.cn/banner_video.mp4 ',
          id: 1,
          pic: ''
        }
      ]
    }
  },
  mounted () {
    this.initVideo()
  },
  methods: {
    async initVideo () {
      var userName = Cookies.get('userName')
      var params = new URLSearchParams()
      params.append('userName', userName)
      await axios
        .post('http://localhost:8081/index.php/index/index/playVideo/', params)
        .then(function (response) {
          count = response.data.data.count
          video_list = response.data.data.video_list
        })
        .catch(function (error) {
          console.log(error)
        })
      var i = 0
      for (;i < count; i++) {
        var src = 'http://localhost:8081/uploads/' + video_list[i]
        console.log(src)
      }
      //  var i = 0;
      // for(;i<count;i++){
      //   var promise = {
      //     'src' : 'http://localhost:8081/uploads/'+video_list[i],
      //     'id'  : i,
      //     'pic' : ""
      //   };
      //   this.list.push(promise);
      // }
      // 初始化视频方法 循环列表获取每个视频的id
      this.list.map((item, i) => {
        // let myPlayer = this.$video('myVideo' + item.id, {
        //   // 确定播放器是否具有用户可以与之交互的控件。没有控件，启动视频播放的唯一方法是使用autoplay属性或通过Player API。
        //   controls: true,
        //   // 自动播放属性,muted:静音播放
        //   autoplay: 'muted',
        //   // 建议浏览器是否应在<video>加载元素后立即开始下载视频数据。
        //   preload: 'auto',
        //   // 设置视频播放器的显示宽度（以像素为单位）
        //   width: '800px',
        //   // 设置视频播放器的显示高度（以像素为单位）
        //   height: '400px',
        //   // 封面图
        //   poster: item.pic
        // })
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
