<script lang="ts">
	import InstallButton from './InstallButton.svelte';
	import coinsIcon from '../lib/images/coins.png';
</script>

<navbar>
	<div class="navbar bg-base-100 px-6 py-3">
		<div class="navbar-start">
			<button class="btn btn-ghost normal-case text-xl"
				>splitty <img src={coinsIcon} alt="download" class="ml-3 h-8" /></button
			>
		</div>
		<div class="navbar-center">
			<div />
		</div>
		<div class="navbar-end">
			<InstallButton />
		</div>
	</div>
</navbar>
