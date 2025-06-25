<script lang="ts">
	import { T } from '@threlte/core';
	import { interactivity, OrbitControls, RoundedBoxGeometry } from '@threlte/extras';
	import { Spring } from 'svelte/motion';

	interactivity();
	let cubes: Spring<number>[][] = Array.from({ length: 5 }, () =>
		Array.from({ length: 5 }, () => new Spring(1))
	);
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
			<T.MeshStandardMaterial color={`hsl(${Math.random() * 360}, 70%, 60%)`} />
		</T.Mesh>
	{/each}
{/each}

<T.Mesh rotation.x={-Math.PI / 2}>
	<T.PlaneGeometry args={[15, 15]} />
	<T.MeshStandardMaterial color="#222222" />
</T.Mesh>
