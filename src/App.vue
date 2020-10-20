<template>
  <div class='app'>
    <button @click="makeMeme">Meme</button>
    <!-- Search Bar -->
    <div class="choices">
      <div class="choiceone">
        <p v-if="choice">{{firstImage.name}}</p>
        <img v-if="choice" id="art" :src='firstImage.image'> 
      </div>
      <div class="choiceone">
        <p v-if="choicetwo">{{secondImage.name}}</p>
        <img v-if="choicetwo" id="art" :src='secondImage.image'> 
      </div>
    </div>
    <div class="search">
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
          <img id="art" :src='fill.image'> 
          <p id="songname">{{fill.name}}</p>
        </div>
      </div>
    </div>

  <!-- Content -->
  <div class="content">
    <div class='image-container' :class="{hide : !hidden}">           
      <canvas id="my_canvas" height=500 width=350 ></canvas>
    </div>
    
    <div class="image-container" :class="{hide: hidden}" >
      <img  id='meme' src="./assets/meme1.jpg">
    </div>
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
      choicetwo: false
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
        context.drawImage(imageOne, -10, 380, 115, 115)
        context.drawImage(imageTwo, 155, 305, 110, 110)
        this.autofill = []
    }
  }
}
</script>

<style>

.content{
  display: flex;
  margin: 15px;
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
  background-color: #a0a0a0
}

.suggestions{
  display: flex;
  padding: 3px;
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
  margin-left: auto;
  margin-right: auto;
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

input{
  width: 65vw;
  margin: 4;
  padding: 4px;
}

button{
  width: 25vw;
  padding: 3px;
}

</style>
