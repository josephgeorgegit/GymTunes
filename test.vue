<template>
    <div class="test">
        <!-- <div class="content">
            <button @click="makeMeme">Sesh</button>
            <div class='canvas-container'>           
                <canvas id="my_canvas" width=300 height=450></canvas>
            </div>
        </div>

        <img id="meme" src="./src/assets/meme1.jpg" alt="">
        <img id="foo" src="./src/assets/WastingLight.jpg" alt=""> -->
        <button @click="makeMeme">Sesh</button>
        <img id="reign" src="./src/assets/Reign.jpg" alt="">
        <div class='canvas-container'>           
                <canvas id="my_canvas" width=300 height=450></canvas>
            </div>



    </div>
</template>

<script>
export default {
    name: "test",
    data(){
        return{

        }
    },
    methods: {
        makeMeme(){
            var skew_canvas = document.createElement("canvas")
            skew_canvas.setAttribute("id", "skew_can")
            var skew_can = skew_canvas.getContext("2d")

            var reign = document.getElementById("reign")
            var nor = new Image()
            nor.src = reign.src
            nor.style.clip = "rect(0px, 45px, 100px, 0px)";
            var skew = new Image()
            skew.src = reign.src

            skew_can.transform(1, -0.5, 0, 1, 0, 0);
            skew_can.drawImage(nor, 1, 330, 90, 90)
            skew_can.drawImage(skew, 1, 330, 90, 90)


            var my_canvas = document.getElementById("my_canvas")
            var context = my_canvas.getContext("2d")
            var meme = document.getElementById("meme")
            var foo = document.getElementById("foo")
            context.drawImage(meme, 0, 0, 300, 420)
            context.drawImage(foo, 132.7, 257, 91, 91)
        }
    }
}
</script>

<style>



#reign{
    height: 90px;
    width: 90px;
    position: absolute;
    clip: rect(0px, 45px, 100px, 0px);
}


</style>