<template>
<div id="app">
  <!-- <button @click='getFlower'>点击加载请求</button> -->
  <ul>
      <li v-for ="item in flowers" :key="item.EnName">
          <img :src="item.EnName" alt="flowers" @click="testPrint(item.EnName)">
          <p>{{item.CnName}}</p>
      </li>
  </ul>
</div>
</template>

<script>
// 引入axios，用于发送请求，defaults 设置后台请求地址
import Cookies from 'js-cookie'
import axios from 'axios'
axios.defaults.baseURL = 'http://localhost:8081/index.php/index/index'
var count = -1
var video_title = []
var video_poster = []

export default {
  data () {
    return {
      flowers: []
    }
  },
  mounted: function () {
    this.getFlower()
  },
  methods: {
    testPrint (x) {
      console.log('this is print')
      console.log(x)
    },
    async getFlower () {
      var userName = Cookies.get('userName')
      var params = new URLSearchParams()
      params.append('userName', userName)
      await axios.post('/playPoster', params)
        .then(function (response) {
          video_title = response.data.data.video_title
          video_poster = response.data.data.video_poster
          count = response.data.data.count
        })
        .catch(function (error) {
          console.log(error)
        })
      var temp_list = []
      var i = 0
      for (;i < count; i++) {
        temp_list.push(
          {
            'EnName': 'http://localhost:8081/uploads/' + video_poster[i],
            'CnName': video_title[i]
          }
        )
      }
      console.log(temp_list)
      this.flowers = temp_list
    }
  }
}
</script>
