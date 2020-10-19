<template>
  <div class='app'>
    
    <!-- Search Bar -->
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
        @click='makeMeme(fill.image)'>
          <img id="art" :src='fill.image'> 
          <p>{{fill.name}}</p>
        </div>
      </div>
    </div>

  <!-- Content -->
  <div class="content">
    <div class='canvas-container' :class="{hide : !hidden}">           
      <canvas id="my_canvas" width=300 height=300></canvas>

    </div>
    
    <div :class="{hide: hidden}" class="image-container">
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
      firstImage: null,
      secondImage: null
    }
  },
  methods: {
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
    //Make a fucken meme g
    makeMeme(album_art){
      this.hidden = true
      var my_canvas = document.getElementById("my_canvas")
      var context = my_canvas.getContext("2d")
      var meme = document.getElementById("meme")
      var art = new Image()
      art.src = album_art
      context.drawImage(meme, 0, 0, 300, 300)
      context.drawImage(art, 132.7, 180, 90, 75)
      this.autofill = []
    }
  }
}
</script>

<style>

.content{
  display: flex;
  margin-top: 10%;
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
  z-index: 99;
}

.suggestions:nth-child(even){
  background-color: #c0c0c0
}

.suggestions:nth-child(odd){
  background-color: #a0a0a0
}

.suggestions{
  display: flex;
  z-index: 99;
  padding: 3px;
}

#art{
  width: 7vw;
  height: 7vh;
}

#art:hover{
  border: 3px solid blue;
}

.image-container{
  margin: auto;
  position: fixed;
}

#meme{
  width: 70vw;
  display: block;
  margin-left: auto;
  margin-right: auto;
  border: 2px solid black;
}

.canvas-container{
  margin: auto;
}

#my_canvas{
  width: 70vw;
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
