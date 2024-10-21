<script>
    import { onMount } from 'svelte';
    export let pixelData = '';
    let canvasColorData;
    let canvas, dragger;
    let canvasHeight = 320;
    let canvasWidth = 640;
    let selectedColor = 'rgb(255, 0, 0)';
    let draggerPosLeft = 50, draggerPosTop = 50;
    let isDragging = false;
    onMount(()=>{
        updateColor();
    });
    $: if(pixelData) handlePropchange();

    function handlePropchange()
    {
        selectedColor = `rgb(${pixelData[0]}, ${pixelData[1]}, ${pixelData[2]})`;
        updateColor();
    }

    function updateColor()
    {
        let c = canvas.getContext('2d');
        let gradientX = c.createLinearGradient(0, 0, canvasWidth, 0);
        gradientX.addColorStop(0, 'white');     // Full white on the left
        gradientX.addColorStop(1, selectedColor);   // color on the right

        // Apply the gradient
        c.fillStyle = gradientX;
        c.fillRect(0, 0, canvasWidth, canvasHeight);

        // Create another linear gradient (top to bottom) for brightness (color to black)
        let gradientY = c.createLinearGradient(0, 0, 0, canvasHeight);
        gradientY.addColorStop(0, 'transparent'); // Transparent at the top
        gradientY.addColorStop(1, 'black');       // Black at the bottom

        // Apply the brightness gradient on top
        c.fillStyle = gradientY;
        c.fillRect(0, 0, canvasWidth, canvasHeight);
    }

    function handleMouseUp(e){
        isDragging = false;
        updatePixelData();
    };

    function updatePixelData()
    {
        const ctx = canvas.getContext('2d');
        const rectDragger = dragger.getBoundingClientRect();
        const x = Math.floor(draggerPosLeft + rectDragger.width/2);
        const y = Math.floor(draggerPosTop + rectDragger.height/2);
        canvasColorData = ctx.getImageData(x, y, 1, 1).data;
        console.log(canvasColorData);
    }

    function updatesliderLocTop(e)
    {
        const rectCanvas = canvas.getBoundingClientRect();
        const rectDragger = dragger.getBoundingClientRect();
        let new_pos = e.clientY - rectCanvas.top;
        
        if(new_pos >= rectCanvas.top && new_pos <= rectCanvas.height - rectDragger.height)
            draggerPosTop = new_pos;
        else if (new_pos < rectCanvas.top)
            draggerPosTop = 0;
        else
            draggerPosTop = rectCanvas.height - rectDragger.height;
    }

    function updatesliderLocLeft(e)
    {
        const rectCanvas = canvas.getBoundingClientRect();
        const rectDragger = dragger.getBoundingClientRect();
        let new_pos = e.clientX - rectCanvas.left;
        
        if(new_pos >= rectCanvas.left && new_pos <= rectCanvas.width - rectDragger.width)
            draggerPosLeft = new_pos;
        else if (new_pos < rectCanvas.left)
            draggerPosLeft = 0;
        else
            draggerPosLeft = rectCanvas.width - rectDragger.width;
    }

    function handleMouseDown(e)
    {
        isDragging = true;
        updatesliderLocLeft(e);
        updatesliderLocTop(e);
        updatePixelData();
    }

    function handleMouseMove(e)
    {
        if(isDragging)
        {
            updatesliderLocLeft(e);
            updatesliderLocTop(e);
            updatePixelData();
        }
    }
</script>

<div style="position: relative; width: 40rem; height: 20rem;">
    <canvas bind:this = {canvas} id="cx" width = {canvasWidth} height = {canvasHeight} on:mousedown={handleMouseDown} on:mousemove={handleMouseMove} on:mouseup={handleMouseUp}></canvas>
    <div bind:this = {dragger} id="dragger" style="left:{draggerPosLeft}px; top:{draggerPosTop}px"></div> 
</div>

<style>
#dragger{
    position: absolute;
    width: 1rem;
    height: 1rem;
    border-radius: 1rem;
    background-color: black;
    border: 1px solid white;
    pointer-events: none;
}
</style>