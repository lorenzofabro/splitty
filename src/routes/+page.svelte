<script>
	import { editionMode } from '../store';
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<section class="mt-5">
	<!-- edition bar -->
	{#if $editionMode}
		<div class="card w-full bg-base-300 shadow-xl">
			<div class="card-body flex flex-row p-2">
				{#each ['card', 'text', 'bubble', 'countdown', 'stats', 'picture', 'alert', 'code'] as value}
					<div class="btn btn-ghost normal-case text-lg">
						{value}
					</div>
				{/each}
			</div>
		</div>
	{/if}

	<!-- blank spaces -->
	<div class="grid grid-rows-4 grid-flow-col gap-4 my-10">
		{#each Array(16) as _, i}
			{#if [0, 1, 2, 3, 5, 7, 10].includes(i)}
				<div class="indicator w-full">
					{#if $editionMode}
						<span class="indicator-item badge badge-sm h-5 w-5 hover:cursor-pointer">X</span>
					{/if}
					<div class="card w-full h-40 bg-base-300 shadow-xl">
						<div class="card-body flex flex-row">{i + 1}</div>
					</div>
				</div>
				<!-- <div class="card w-full h-40 bg-base-300 shadow-xl">
					<div class="card-body flex flex-row">{i + 1}</div>
				</div> -->
			{:else if $editionMode}
				<div class="card w-full h-40 bg-base-100 border-2 border-dashed">
					<div class="card-body flex flex-row">{i + 1}</div>
				</div>
			{:else}
				<div />
			{/if}
		{/each}
	</div>
</section>
