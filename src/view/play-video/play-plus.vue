<template>
  <md-card>
      <md-card-media>
          <div v-for="(item,i) in this.playerOptions.sources" :key="i" style="margin-bottom: 30px">
              <div class="item">
                <div class="player">
                <video-player  class="vjs-custom-skin"
                                ref="videoPlayer"
                                :options="playerOptions.sources[i]"
                                :playsinline="true">
                                <!-- <source :src="item.src" type="video/mp4"> -->
                </video-player>
                </div>
            </div>
          </div>
            <!-- <div class="item">
                <div class="player">
                <video-player  class="vjs-custom-skin"
                                ref="videoPlayer"
                                :options="playerOptions"
                                :playsinline="true">
                </video-player>
                </div>
            </div> -->
    </md-card-media>
    <!-- <md-card-media>
      <div class="item">
        <div class="player">
          <video-player  class="vjs-custom-skin"
                         ref="videoPlayer"
                         :options="playerOptions"
                         :playsinline="true">
          </video-player>
        </div>
      </div>
    </md-card-media> -->
    <!-- <div v-for="(item,i) in this.playerOptions.sources" :key="i" style="margin-bottom: 30px">
      <video :id="'myVideo'+item.id" class="video-js">
        <source :src="item.src" type="video/mp4">
      </video>
    </div> -->
  </md-card>
</template>

<script>

export default {
  data () {
    return {
      playerOptions: {
        height: '360',
        autoplay: true,
        muted: true,
        language: 'en',
        playbackRates: [0.7, 1.0, 1.5, 2.0],
        sources: [{
          type: 'video/mp4',
          src: 'http://vjs.zencdn.net/v/oceans.mp4'
        }],
        poster: 'https://surmon-china.github.io/vue-quill-editor/static/images/surmon-1.jpg'
      }
    }
  },
  mounted () {
    // console.log('this is current player instance object', this.player)
    setTimeout(() => {
      this.playerOptions.sources = [{
        type: 'video/mp4',
        src: 'http://localhost:8081/uploads/20200303/6459e3a42b50e2125cd41666c4b1152a.mp4'
      }, {
        type: 'video/mp4',
        src: 'http://localhost:8081/uploads/20200303/ff6f61580d9cdc2fbaab78a8c6bb3e84.mp4'
      }]
      // console.log('dynamic change options', this.player)
      // this.playerOptions.sources[0].src = 'http://localhost:8081/uploads/20200303/ff6f61580d9cdc2fbaab78a8c6bb3e84.mp4';
      // change src
      // this.playerOptions.sources[0].src = 'https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm';
      // change item
      // this.$set(this.playerOptions.sources, 0, {
      //   type: "video/mp4",
      //   src: 'https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm',
      // })
      // change array
      // this.playerOptions.sources = [{
      //   type: "video/mp4",
      //   src: 'https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm',
      // }]
      this.player.muted(false)
    }, 5000)
  },
  computed: {
    player () {
      return this.$refs.videoPlayer.player
    }
  },
  methods: {
  }
}
</script>
