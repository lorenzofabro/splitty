<script lang="ts">
	import { onMount } from 'svelte';
	import cloudIcon from '../lib/images/cloud.png';

	let deferredInstallEvent: any;

	onMount(() => {
		window.addEventListener('beforeinstallprompt', (e) => {
			e.preventDefault();
			deferredInstallEvent = e;
		});
	});

	async function handleInstall() {
		deferredInstallEvent.prompt();
		let choice = await deferredInstallEvent.userChoice;
		if (choice.outcome === 'accepted') {
			// User accepted to install the application
		} else {
			// User dismissed the prompt
		}
		deferredInstallEvent = undefined;
	}
</script>

{#if deferredInstallEvent}
	<button class="btn btn-ghost normal-case text-xl" on:click={handleInstall}
		>install <img src={cloudIcon} alt="download" class="ml-3 h-8" /></button
	>
{/if}

<slot />
