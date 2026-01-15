<script lang="ts">
	import { T } from '@threlte/core';
	import { interactivity, OrbitControls, RoundedBoxGeometry } from '@threlte/extras';
	import { onMount } from 'svelte';
	import { Spring } from 'svelte/motion';

	interactivity();
	let r = 0;
	let d = 1;

	let cubes: Spring<number>[][] = Array.from({ length: 5 }, () =>
		Array.from({ length: 5 }, () => new Spring(1))
	);

	function update() {
		r += d;

		if (r > 360) {
			d = -1;
		}

		if (r < 0) {
			d = 1;
		}

		setTimeout(() => {
			update();
		}, 1);
	}

	onMount(update);
</script>

<T.PerspectiveCamera
	makeDefault
	position={[10, 15, -10]}
	oncreate={(ref) => {
		ref.lookAt(0, 0, 0);
	}}
>
	<OrbitControls enableDamping />
</T.PerspectiveCamera>

<T.DirectionalLight position={[0, 10, 10]} intensity={1.2} castShadow />

<T.AmbientLight color="#d6efff" />

{r}

{#each cubes as row, x}
	{#each row as cube, z}
		<T.Mesh
			position.y={cube.current - 0.5}
			position.x={x * 2.25 - 4.5}
			position.z={z * 2.25 - 4.5}
			castShadow
			scale.y={cube.current}
			onpointerenter={() => (cube.target = 0.7)}
			onpointerleave={() => (cube.target = 1)}
		>
			<RoundedBoxGeometry radius={0.5} args={[2, 2, 2]} />
			<T.MeshStandardMaterial color={`hsl(${r}, 100%, 70%)`} />
		</T.Mesh>
	{/each}
{/each}

<T.Mesh rotation.x={-Math.PI / 2}>
	<T.PlaneGeometry args={[15, 15]} />
	<T.MeshStandardMaterial color="#222222" />
</T.Mesh>
