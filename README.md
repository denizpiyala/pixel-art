hii I'm tired created that little pixel art game .
I hope you like it :) 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>pixel art</title>
    <link rel="stylesheet" href="style.css">
    <style>
        *{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Courier New', Courier, monospace;
}

body{
    background-color: purple;
}

.wrapper{
    background-color: #fff;
    width: 80vmin;
    position: absolute;
    transform: translate(-50%,-50%);
    top: 50%;
    left: 50%;
    padding: 40px 20px;
    border-radius: 8px;
}

label{
    display: block;
}

span{
    position: relative;
    font-size:22px;
    bottom: -1px;
}

.opt-wrapper{
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    gap: 10px;
}

button{
    background-color: #76a9bc;
    border: none;
    border-radius: 6px;
    padding: 6px;
    color: #fff;
}

input[type="color"]{
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    background-color: transparent;
    width: 70px;
    height: 40px;
    border: none;
    cursor: pointer;
}

input[type="color"]::-webkit-color-swatch{
    border-radius: 10px;
    border:5px-solid #000;
}
input[type="color"]::-moz-color-swatch{
    border-radius: 10px;
    border:5px-solid #000;
}

.gridCol{
    height: 1em;
    width: 1em;
    border: 1px solid #ddd;
}

.gridRow{
    display: flex;
}

@media only screen and (max-width:768px){
    .gridCol{
        height: 0.8em;
        width: 0.8em;
    }
}
    </style>
  
    
</head>
<body>
    <div class="wrapper">
        <div class="options">
            <div class="opt-wrapper">
                <div class="slider">
                    <label for="width-range">Grid width</label>
                    <input type="range" id="width-range" min="1"
                    max="35">
                    <span id="width-value">00</span>
                </div>
                <div class="slider">
                    <label for="height-range">Grid width</label>
                    <input type="range" id="height-range" min="1"
                    max="35">
                    <span id="height-value">00</span>
                </div>
            </div>
            <div class="opt-wrapper">
                <button id="submit-grid">Create Grid</button>
                <button id="clear-grid">Clear Grid</button>
                <input type="color" id="color-input">
                <button id="erase-button">Erase</button>
                <button id="paint-button">Paint</button>
            </div>

        </div>
        <div class="container"></div>
    </div>
 <script>
    let container = document.querySelector(".container");
let gridButton = document.getElementById("submit-grid");
let clearGridButton = document.getElementById("clear-grid");
let gridWidth = document.getElementById("width-range");
let gridHeight = document.getElementById("height-range");
let colorButton = document.getElementById("color-input");
let erasebutton = document.getElementById("erase-button");
let paintbutton = document.getElementById("paint-button");
let widthValue = document.getElementById("width-value");
let heightValue = document.getElementById("height-value");

let erase = false;

let events = {
    mouse: {
        down: "mousedown",
        move: "mousemove",
        up: "mouseup",
    },
    Touch: {
        down: "touchstart",
        move: "touchmove",
        up: "touchend",
    },
};

let deviceType = "";


const isTouchDevice = () => {
    try {
        document.createEvent("TouchEvent");
        deviceType = "Touch";
        return true;
    } catch (e) {
        deviceType = "mouse";
        return false;
    }
};

isTouchDevice();

gridButton.addEventListener("click", () => {
    container.innerHTML = "";
    let count = 0;
    for(let i = 0 ; i < gridHeight.value; i++) {
        count += 2;
        let div = document.createElement("div");
        div.classList.add("gridRow");

        for (let j = 0; j < gridWidth.value; j++) {
            count+= 2;
            let col = document.createElement("div");
            col.classList.add("gridCol");
            col.setAttribute("id",`gridCol${count}`);
            col.addEventListener(events[deviceType].down, ()=> {
                 draw = true;
                if(erase){
                    col.style.backgroundColor = "transparent";
                } else {
                    col.style.backgroundColor = colorButton.value;
                }
            });
            col.addEventListener(events[deviceType].move,(e) => {
                let ElementId = document.elementFromPoint(
                    !isTouchDevice() ? e.clientX : e.touches[0].
                    clientX,
                    !isTouchDevice() ? e.clientY : e.touches[0].
                    clientY
                ).id;
                checker(ElementId);
            });

            col.addEventListener(events[deviceType].up, ()=>{
                draw = false;
            });
            div.appendChild(col);
        }
        container.appendChild(div);
    }
});

function checker(ElementId) {
    let gridColums = document.querySelectorAll(".gridCol");
    gridColums.forEach((element) => {
        if(ElementId == element.id) {
            if (!erase){
                element.style.backgroundColor = colorButton.value;
            }else{
                element.style.backgroundColor = "transparent";
            }
        }
    });
}

clearGridButton.addEventListener("click", ()=> {
    container.innerHTML = "";
});

erasebutton.addEventListener("click", ()=> {
    erase = true ;
});

paintbutton.addEventListener("click", () => {
    erase = false;
});

gridWidth.addEventListener("input", () => {
    widthValue.innerHTML = gridWidth.value < 9 ? `0${gridWidth.value}` :gridWidth.value;
});

gridHeight.addEventListener("input", () => {
    heightValue.innerHTML = gridHeight.value < 9 ? `0${gridHeight.value}` :gridHeight.value;
});

window.onload = () => {
    gridHeight.value = 0;
    gridWidth.value = 0;
};
 </script>
</body>
</html>
