<script lang="ts">
	let name: string = '';
	let amount: number = 0;

	let persons: any[] = [];
	const addPerson = () => {
		persons = [...persons, { name: name, amount: amount }];

		name = '';
		amount = 0;
	};

	const remove = (person: any) => {
		persons = persons.filter((i) => i !== person);
	};
</script>

<svelte:head>
	<title>splitty</title>
</svelte:head>

<section class="mt-5">
	<div class="flex">
		<div class="flex-auto w-64">
			<form on:submit|preventDefault={addPerson}>
				<input
					type="text"
					placeholder="name"
					class="input input-bordered w-full max-w-xs"
					bind:value={name}
					required
				/>
				<input
					type="number"
					placeholder="amount"
					class="input input-bordered w-full max-w-xs"
					bind:value={amount}
					required
				/>
				<button class="btn">add</button>
			</form>
		</div>
		<div class="flex-auto w-32 justify-end">
			<button class="btn">calculate</button>
		</div>
	</div>
	<ul>
		{#each persons as person}
			<li>
				<span>{person.name}</span>
				<span>{person.amount}</span>
				<button on:click={() => remove(person)}>&times;</button>
			</li>
		{/each}
	</ul>
</section>
