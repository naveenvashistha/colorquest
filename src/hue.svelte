<script>
    import { onMount } from 'svelte';
    onMount(()=>{
        const canvas = document.getElementById('cy');
        const c = canvas.getContext('2d');
        const width = canvas.width;
        const height = canvas.height;

        const hueGradient = c.createLinearGradient(0, 0, width, 0);

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
        c.fillRect(0, 0, width, height);
    });
</script>

<div>
    <canvas id="cy">
        <div class="slide-bar"></div>
    </canvas>
</div>

<style>
#cy{
    width: 40rem;
    height: 1rem;
}

.slide-bar{
    height: 1rem;
    background-color: grey;
    width: 0.25rem;
}
</style>