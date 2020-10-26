<template>
  <div class='app'>
    <div v-if="!showfinal">
      <p id="message" v-if="!twoImages">Whats your pre-workut music? Select 2 albums, screenshot and share!</p>

      <!-- Search Bar -->
      <div v-if="!twoImages" class="search">
          <input 
          v-model='query'
          @keypress="funk" 
          type="text"
          placeholder="Enter Album Name">
          <button  @click="funk">Search</button>
      </div>

    <!-- Suggestions -->
      <div class="suggestions-container" >
        <div v-if="query != null">
          <div 
          class='suggestions' 
          v-for="fill in autofill" 
          :key='fill.id'    
          @click='selectImage(fill)'>
          <div>
            <img id="art" :src='fill.image'> 
          </div>
            <div class="songinfo">
              <p id="songname">{{fill.name}}</p>
              <p id="artistname">{{fill.artist}}</p>
            </div>
          </div>
        </div>
      </div>


    <!-- Choices -->
      <div class="choices">
        <div v-if="choice" class="choiceone">
          <img  id="art" :src='firstImage.image'> 
          <p>{{firstImage.name}}</p>
        </div>
        
        <div v-if="choicetwo" class="choiceone">
          <img  id="art" :src='secondImage.image'> 
          <p >{{secondImage.name}}</p>
        </div>
      </div>

    <!-- meme button -->
      <div v-if="twoImages" class="memebutton">
        <button id="memebutton" v-if="twoImages" @click="makeMeme">Make Meme</button>
      </div>
    </div>
  <!-- Content -->
  <div class="content">
    <div class='image-container' :class="{hide : !hidden}">           
      <canvas id="my_canvas" height=500 width=350 ></canvas>
    </div>
    
    <div class="image-container" :class="{hide: hidden}" >
      <img  id='meme' src="./assets/meme.png">
    </div>
  </div>

  <!-- Built With -->
  <div v-if="!showfinal" class="builtwith">
    <p>Built with Vue.js and Last.fm</p>
  </div>

  <!-- Shameless Plug -->
  <div class="plug" v-if="showfinal">
    <p>Screenshot, share & follow <a href="https://instagram.com/youssoundgood">@youssoundgood</a> on instagram</p>
  </div>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return{
      api_key: '710afa9401c18d5c62a9259faf121bab',
      lastfm_url: 'https://ws.audioscrobbler.com/2.0/',
      query: null,
      data: {},
      autofill: [],
      hidden: false,
      firstImage: '',
      secondImage: "",
      total: 1,
      choice: false,
      choicetwo: false,
      twoImages: false,
      showfinal: false
    }
  },
  methods: {
    log(){
      console.log("log")
    },
    //fetch data and store it
    funk(){
      this.feedback = false
      setTimeout(() => {
        fetch(`${this.lastfm_url}?method=album.search&album=${this.query}}&limit=5&api_key=710afa9401c18d5c62a9259faf121bab&format=json`)
        .then( res => {
          return res.json()
        }).then(
          this.setResutlts
          )}, 1000)
        },
    setResutlts(results){
      this.data = results
      var i 
        this.autofill = []
        for(i = 0; i< 5; i++){
          this.autofill.push({
            "image" : this.data.results.albummatches.album[i].image[2]["#text"],
            "name": this.shortenResult(this.data.results.albummatches.album[i].name),
            "artist": this.shortenResult(this.data.results.albummatches.album[i].artist),
            "id": i })
          }
    },
    //shorten autofill results
    shortenResult(el){
      if(el.length > 45){
        return el.substring(0, 45)
      }else{
        return el
      }
    },
    selectImage(album_art){
      if(this.total == 1){
        this.firstImage = album_art
        this.total++
        console.log("first Image")
        this.autofill = []
        this.query = null
        this.choice = true
      }else{
        this.secondImage = album_art
        console.log("second Image")
        this.autofill = []
        this.query = null
        this.choicetwo = true
        this.twoImages = true
      }
    },
    //Make a fucken meme g
    makeMeme(){
      this.choice = false
      this.choicetwo = false
      console.log("hello")
        this.hidden = true
        var my_canvas = document.getElementById("my_canvas")
        var context = my_canvas.getContext("2d")
        var meme = document.getElementById("meme")
        var imageOne = new Image()
        imageOne.style.height = "50vh"
        var imageTwo = new Image()
        imageOne.src = this.firstImage.image
        imageTwo.src = this.secondImage.image
        context.drawImage(meme, 0, 0, 350, 500 )
        context.drawImage(imageOne, 0, 370, 115, 115)
        context.drawImage(imageTwo, 168, 305, 100, 100)
        this.autofill = []
        this.showfinal = true
    }
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@200&display=swap");

div .memebutton{
  text-align: center;
}

button{
  background-color: #f0f0f0; /* Green */
  border: 1px #a0a0a0 solid;
  text-align: center;
  display: inline-block;
  font-size: 16px;
  width: 25vw;
  padding: 2px;
}

input{
  border: 1px #a0a0a0 solid;
  border-radius: 0%;
  width: 65vw;
  margin: 4;
  padding: 4px;
}

.songinfo{
  margin-left: 4px;
}
.builtwith{
  text-align: center;
}

.plug{
  text-align: center;
}

.choices{
  display: flex;
  justify-content:space-evenly;
}

#artistname{
  opacity: 0.6
}

#message{
  text-align: center;
}

.app{
  max-width: 100vw;
  font-family: 'Montserrat', sans-serif;
}
.content{
  display: flex;
}

.serach-items{
  border: 1px solid #d4d4d4;
  border-bottom: none;
  border-top: none;
  z-index: 99;
  top: 100%;
  left: 0;
  right: 0;
}

.hide{
    display: none;
}

.suggestions-container{
  display: inline-block;
}

.suggestions:nth-child(even){
  background-color: #e0e0e0
}

.suggestions:nth-child(odd){
  background-color: #d0d0d0
}

.suggestions{
  padding: 4px;
  display: flex;
  margin: 1px;
  min-width: 85vw;
  transition: 0.2s;
}
.suggestions:hover{
  opacity: 0.6;
  transition: 0.2s;
}

.image-container{
  margin: auto;
  margin-top: 10px;
}

#art{
  opacity: 1;
  width: auto;
  height: 75px;
  transition: 0.2s;
}

#art:hover{
  opacity: 0.6;
  transition: 0.2s
}

#meme{
  height: 500px;
  width: 350px;
  display: block;
  border: 2px solid black;
}

#my_canvas{
  height: 400;
  width: 235;
  display: block;
  margin-left: auto;
  margin-right: auto;
  border: 2px solid black;
}

.search{
  position: relative;
  display: block;
  margin: 4px;
}
</style>
