<script>
    import { onMount } from 'svelte';
    import Canvas from './canvas.svelte';
  import { callApi } from './stores';
    let slider, hue, isDragging = false, pixelData;
    let sliderPos = 0;
    let canvasWidth = 640;
    let canvasHeight = 32;
    onMount(()=>{
        const c = hue.getContext('2d');
        
        const hueGradient = c.createLinearGradient(0, 0, canvasWidth, 0);

        // Add all colors from the hue spectrum
        hueGradient.addColorStop(0, 'rgb(255, 0, 0)');    // Red
        hueGradient.addColorStop(1 / 6, 'rgb(255, 255, 0)'); // Yellow
        hueGradient.addColorStop(2 / 6, 'rgb(0, 255, 0)');   // Green
        hueGradient.addColorStop(3 / 6, 'rgb(0, 255, 255)'); // Cyan
        hueGradient.addColorStop(4 / 6, 'rgb(0, 0, 255)');   // Blue
        hueGradient.addColorStop(5 / 6, 'rgb(255, 0, 255)'); // Magenta
        hueGradient.addColorStop(1, 'rgb(255, 0, 0)');       // Back to Red

        // Draw the gradient on the canvas
        c.fillStyle = hueGradient;
        c.fillRect(0, 0, canvasWidth, canvasHeight);
        const x = Math.floor(sliderPos);
        const y = Math.floor(canvasHeight / 2);
        pixelData = c.getImageData(x, y, 1, 1).data;
    });

    document.addEventListener("mouseup", handleMouseUp);
    document.addEventListener("mousemove", handleMouseMove);

    function handleMouseUp(e){
        if(isDragging)
        {
            isDragging = false;
            $callApi = true;
            updatesliderLoc(e);
        }
    };

    function updatePixelData()
    {
        const ctx = hue.getContext('2d');
        const x = Math.floor(sliderPos);
        const y = Math.floor(canvasHeight / 2);
        pixelData = ctx.getImageData(x, y, 1, 1).data;
    }

    function updatesliderLoc(e)
    {
        const rectHue = hue.getBoundingClientRect();
        const rectSlider = slider.getBoundingClientRect();
        let new_pos = e.clientX - rectHue.left;
        
        if(new_pos >= 0 && new_pos <= rectHue.width - rectSlider.width)
            sliderPos = new_pos;
        else if (new_pos < 0)
            sliderPos = 0;
        else
            sliderPos = rectHue.width - rectSlider.width;
        updatePixelData();
    }

    function handleMouseDown(e)
    {
        isDragging = true;
    }

    function handleMouseMove(e)
    {
        if(isDragging)
            updatesliderLoc(e);
    }
    
</script>
<div>
    <Canvas pixelData = {pixelData}/>
    <div id="hue-container"> 
        <canvas bind:this = {hue} id="hue" on:mousedown={handleMouseDown}  width = {canvasWidth} height = {canvasHeight}></canvas>
        <div bind:this = {slider} id="slider" style="left:{sliderPos}px;"></div> 
    </div>
</div>
<style>
#hue-container{
    position: relative;
    align-items: center;
    width: 40rem;
    height: 2rem;
    margin-top: 2rem;
}
#hue{
    border: 1px solid white;
}

#slider{
    height: 32px;
    position: absolute;
    background-color: black;
    width: 4px;
    top: 0;
    pointer-events: none;
}
</style>