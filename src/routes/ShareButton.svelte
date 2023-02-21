<script lang="ts">
	import { copied } from '../store';
	import { onMount } from 'svelte';

	export let text: string;

	let canShare: boolean;

	onMount(async () => {
		canShare = navigator.canShare;
	});

	async function handleClick() {
		try {
			if (canShare) {
				await navigator.share({ text });
			} else {
				await navigator.clipboard.writeText(text);
				copied.set(true);
			}
		} catch (error) {
			console.log(error);
		}
	}
</script>

<button class="btn" on:click={handleClick}>
	{#if canShare}
		<slot>share</slot>
	{:else if $copied}
		<slot name="copied">copied!</slot>
	{:else}
		<slot>copy</slot>
	{/if}
</button>
