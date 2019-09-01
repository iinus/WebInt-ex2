<template>
  <div class="video-player">
    <div class="video-container">
       <h3><br>Now playing: {{this.activeTitle}}</h3>
      <video ref="video" controls :src="activeUrl" type="video/mp4" />
      <div>
        <form class="row">
          <p>
            Jump to offset:<br>
            <input id="jump" v-model="jump" type="number" placeholder="Offset" name="jump" @input="jumpToOffset"/> 
          </p>
          <p><br><button type="button" @click="rotateVideo" >ROTATE</button></p>
          <p>
            Show/hide controls <br>
            <input id="controls" class="checkbox" type="checkbox" @click="showHideControls()" name="controls">
          </p>
        </form>
        <form v-on:submit="loadNewVideo">
          <p v-if="errors.length">
            <b>Please correct the following error(s):</b>
              <li :key="error" v-for="error in errors">{{ error }}</li>
          <p>
            Place your own video-url here: <br>
            <input id="video-url" v-model="url" type="text" name="video-url"><br><br>
          <button type="button" @click="loadNewVideo">SUBMIT</button>
          </p>
        </form>
      </div>
    </div>
    <div class="video-list" >
      <div @click="chooseVideo(video)" :key="video.id" v-for="video in videos" class="thumbnail"> 
        <video>
          <source :src="video.url" :type="video.type"> 
        </video>
        <div class="thumbnail">
          <p> {{video.title}} </p>
        </div>
      </div>    
    </div>
  </div>
</template>

<script>
import '../styles/video.css';

let videos = [
  {
    id: 1,
    title: "Electricsheep flock",
    url: "https://ia801504.us.archive.org/17/items/electricsheep-flock-247-30000-6/00247%3D30016%3D29960%3D29860.mp4",
    type: "video/mp4",
  },
  {
    id: 2,
    title: "BRUDER TOYS SCANIA CAT Construction Company",
    url: "https://ia600105.us.archive.org/15/items/ConcreteMixerTruckAndDumpTruckCraneForKidsVideoUnboxingToyBiBi/BRUDER%20TOYS%20SCANIA%20CAT%20Construction%20Company.mp4",
    type: "video/mp4",
  },
  {
    id: 3,
    title: "Rabbit every Monday",
    url: "https://ia801704.us.archive.org/27/items/RabbitEveryMonday/RabbitEveryMonday.mp4#t=3.0001",
    type: "video/mp4",
  },
  {
    id: 4,
    title: "Knighty Knight Bugs",
    url: "https://ia801700.us.archive.org/31/items/KnightyKnightBugs/001KnightyKnightBugsLt-mythology-folk.mp4#t=3.1001",
    type: "video/mp4",
  }
];

export default {
  name: 'VideoPlayer',
  data () {
    return {
      videos,
      errors:[],
      url:null,
      jump: 0,
      duration: 3,
      rotation: 0,
      activeUrl: videos[0].url,
      activeTitle: videos[0].title
    }
  },
  methods:{
  chooseVideo(video){
    this.activeVideo = video;
    this.activeUrl = this.activeVideo.url;
    this.activeTitle = this.activeVideo.title;
    this.jump = 0;
    this.rotation = 0;
  },
  showHideControls(){
    if (this.$refs.video.controls){
      this.$refs.video.controls=false;
    }
    else{
      this.$refs.video.controls=true;
    }
  },
  jumpToOffset: function(e){
    this.duration = this.$refs.video.duration;
    if (this.jump > this.duration){
      this.jump = this.duration;
    }
    if (this.$refs.video.currentTime != this.jump) {
      this.$refs.video.currentTime = this.jump;
    }
  },
  setDuration(){
    this.duration = this.$refs.video.duration;
    console.log(this.duration);
  },
  rotateVideo: function(e){
    this.rotation = this.rotation + 180;
    this.$refs.video.style.transform = "rotate("+this.rotation+"deg)"
  },
  addLike(){
    this.activeVideo.likes += 1;
  },
  loadNewVideo: function(e){
    e.preventDefault();
    if (this.url != null){
      if (this.validUrl(this.url)){
        this.activeUrl = this.url;
        this.$refs.video.src = this.activeUrl;
        this.activeTitle = "Your video";
      }
      this.errors=[];
      if(!this.validUrl(this.url)){
        this.errors.push("Valid url required.");
      }
      if(!this.url.includes("mp4")){
        this.errors.push("Video must be in mp4 format.");
      }
    } else{
      this.errors.push("Url cannot be empty.");
    }
  },
  validUrl: function(url){
    return /^(?:(?:(?:https?|ftp):)?\/\/)(?:\S+(?::\S*)?@)?(?:(?!(?:10|127)(?:\.\d{1,3}){3})(?!(?:169\.254|192\.168)(?:\.\d{1,3}){2})(?!172\.(?:1[6-9]|2\d|3[0-1])(?:\.\d{1,3}){2})(?:[1-9]\d?|1\d\d|2[01]\d|22[0-3])(?:\.(?:1?\d{1,2}|2[0-4]\d|25[0-5])){2}(?:\.(?:[1-9]\d?|1\d\d|2[0-4]\d|25[0-4]))|(?:(?:[a-z\u00a1-\uffff0-9]-*)*[a-z\u00a1-\uffff0-9]+)(?:\.(?:[a-z\u00a1-\uffff0-9]-*)*[a-z\u00a1-\uffff0-9]+)*(?:\.(?:[a-z\u00a1-\uffff]{2,})))(?::\d{2,5})?(?:[/?#]\S*)?$/i.test(url);
  }
}
}
</script>