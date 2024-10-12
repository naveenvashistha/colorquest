<script>
    import { onMount } from 'svelte';
    onMount(()=>{
        const canvas = document.getElementById('cx');
        const c = canvas.getContext('2d');
        const gradientX = c.createLinearGradient(0, 0, canvas.width, 0);
        gradientX.addColorStop(0, 'white');     // Full white on the left
        gradientX.addColorStop(1, 'red');   // color on the right

        // Apply the gradient
        c.fillStyle = gradientX;
        c.fillRect(0, 0, canvas.width, canvas.height);

        // Create another linear gradient (top to bottom) for brightness (color to black)
        const gradientY = c.createLinearGradient(0, 0, 0, canvas.height);
        gradientY.addColorStop(0, 'transparent'); // Transparent at the top
        gradientY.addColorStop(1, 'black');       // Black at the bottom

        // Apply the brightness gradient on top
        c.fillStyle = gradientY;
        c.fillRect(0, 0, canvas.width, canvas.height);
    });
</script>

<div>
    <canvas id="cx"></canvas>
</div>

<style>
#cx{
    width: 40rem;
    height: 20rem;
}
</style>