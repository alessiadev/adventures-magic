<script>
	import { invoke } from '@tauri-apps/api/tauri';
	import { onMount } from 'svelte';
	import { resolveResource } from '@tauri-apps/api/path';
	import { readTextFile } from '@tauri-apps/api/fs';

	let name = '';
	let greetMsg = '';
	let data = { name: null };

	async function greet() {
		greetMsg = await invoke('greet', { name });
	}
	onMount(async () => {
		const resourcePath = await resolveResource('assets/test.json');
		data = JSON.parse(await readTextFile(resourcePath));
	});
</script>

<div>
	<input id="greet-input" placeholder="Enter a name..." autocomplete="off" bind:value={name} />
	<button on:click={greet}>Greet</button>
	<p>{greetMsg}</p>
	<p>Name: {data.name ?? '-'}</p>
</div>
