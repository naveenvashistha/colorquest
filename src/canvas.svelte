<script>
    import { onMount } from 'svelte';
    export let pixelData = '';
    let canvas;
    let canvasHeight = 320;
    let canvasWidth = 640;
    let selectedColor = 'rgb(255, 0, 0)';
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
</script>

<div>
    <canvas bind:this = {canvas} id="cx" width = {canvasWidth} height = {canvasHeight}></canvas>
</div>

<style>
#cx{
    
}
</style>