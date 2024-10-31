<script>
	import Hue from './hue.svelte';
	import ColorValue from './colorValue.svelte';
</script>

<div class="app-container">
	<Hue />
	<ColorValue />
</div>

<style>
	.app-container{
		display: flex;
		justify-content: space-around;
	}
</style>