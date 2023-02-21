<script lang="ts">
	import { copied } from '../store';

	export let text: string;
	export let url = 'https://splitty-ten.vercel.app/';
	export let title = 'splitty';

	let canShare: boolean;

	async function handleClick() {
		canShare = navigator.canShare;
		try {
			if (canShare) {
				await navigator.share({ text, url, title });
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
