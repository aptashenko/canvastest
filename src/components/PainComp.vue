<template>
    <canvas class="canv" ref="canvas" @mousedown="toggleMouseDown" @mouseup="toggleMouseDown" @mousemove="draw">SAD</canvas>
    <div class="panel">
        <p>Pick a color:</p>
        <ul class="list">
            <li @click="takeColor" v-for="(color, i) of colors" :key="i + 'color'" :style="{backgroundColor: color}"
                class="palitra"></li>
        </ul>
        <p>Choose a brush width:</p>
        <input @input="setBrushWidth" type="range" value="1" min="1" max="20">
        <button class="button" @click="clearCanvas">Clear</button>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue'
export default {
    setup() {
        const colors = ["#FF0000", "#FF8000", "#FFFF00", "#80FF00", "#00FF00", "#00FF80", "#00FFFF", "#0080FF", "#0000FF", "#8000FF", "#FF00FF", "#FF0080"];
        const canvas = ref(null);
        let ctx = null;
        let newColor = ref(null);
        let isMouseDown = ref(false);
        let brushWidth = ref(0);
        const toggleMouseDown = () => {
            isMouseDown.value = !isMouseDown.value;
            if (!isMouseDown.value) {
                ctx.beginPath();
            }
        };
        const setBrushWidth = (e) => {
            brushWidth.value = e.target.value;
        };

        const clearCanvas = () => {
            canvas.value.getContext("2d").clearRect(0,0,500,500);
        }
        const draw = (e) => {
            if (isMouseDown.value) {
                ctx = canvas.value.getContext("2d");
                ctx.strokeStyle = newColor.value;
                ctx.fillStyle = newColor.value;
                ctx.lineWidth = brushWidth.value * 2;
                ctx.lineTo(e.clientX, e.clientY);
                ctx.stroke();
                ctx.beginPath();
                ctx.arc(e.clientX, e.clientY, brushWidth.value, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.moveTo(e.clientX, e.clientY);
            }
        };
        function componentToHex(c) {
            var hex = c.toString(16);
            return hex.length == 1 ? "0" + hex : hex;
        }
        function rgbToHex(r, g, b) {
            return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
        }
        const takeColor = (e) => {
            const backgroundColor = getComputedStyle(e.target).backgroundColor;
            const color = backgroundColor.replace("rgb", "").replace(")", "").replaceAll("(", "");
            const colorArray = color.split(",");
            newColor.value = rgbToHex(Number(colorArray[0]), Number(colorArray[1]), Number(colorArray[2]));
        };
        onMounted(() => {
            canvas.value.width = 500;
            canvas.value.height = 500;
        });
        return { canvas, takeColor, toggleMouseDown, draw, setBrushWidth, colors, clearCanvas };
    },
}
</script>

<style>
.panel {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0 40px;
    border: 1px solid #000;
}

.list {
    list-style: none;
    display: flex;
    width: 160px;
    flex-wrap: wrap;
    padding: 0;
}

.button {
    background: transparent;
    cursor: pointer;
    border-radius: 50px;
    margin-top: 20px;
}

.palitra {
    width: 30px;
    height: 30px;
    margin: 5px;
    cursor: pointer;
}

.red {
    background: red;
}

.blue {
    background: blue;
}

.canv {
    border: 1px solid #000;
    display: block;
}
</style>