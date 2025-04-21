<template>
  <div class="video-player-container">
    <video
      ref="videoPlayer"
      class="video-player"
      :src="currentVideo"
      controls
      autoplay
      muted
      @ended="onVideoEnd">
      Your browser does not support the video tag.
    </video>
  </div>
</template>

<script>
module.exports = {
  data() {
    return {
      currentVideoIndex: 0,
      slides: [],
    };
  },
  computed: {
    currentVideo() {
      return this.slides[this.currentVideoIndex] || "";
    },
  },
  mounted() {
    this.fetchVideos();
  },
  methods: {
    fetchVideos() {
      axios
        .get("http://127.0.0.1:1880/datavideo")
        .then((response) => {
          const baseURL = "http://127.0.0.1:1880/video/";
          this.slides = response.data.map((item) => `${baseURL}${item.name}`);
        })
        .catch((error) => {
          console.error("Error fetching videos:", error);
        });
    },
    onVideoEnd() {
      this.currentVideoIndex++;

      if (this.currentVideoIndex >= this.slides.length) {
        this.currentVideoIndex = 0;
      }

      this.$refs.videoPlayer.load();
      this.$refs.videoPlayer.play();
    },
  },
};
</script>

<style scoped>
.video-player-container {
  margin: 95px auto 0;
  display: flex;
  justify-content: center;
}

.video-player {
  width: 100%;
  height: auto;
  border-radius: 10px;
  overflow: hidden;
}
</style>
