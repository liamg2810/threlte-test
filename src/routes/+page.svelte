<script>
	import Scene from '$lib/Scene.svelte';
	import { Canvas } from '@threlte/core';
</script>

<div>
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	div {
		height: 100vh;
		width: 100vw;
		background-color: rgb(25, 25, 25);
	}
</style>
