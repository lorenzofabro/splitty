<script lang="ts">
	import type { Debt, Person } from './interfaces';
	import PersonCard from './Person.svelte';

	let name: string = '';
	let amount: number = 0;

	let people: Person[] = [];
	let debts: Debt[] = [];

	const addPerson = () => {
		const lastId = people.reduce((maxId, person) => Math.max(maxId, person.id), 0);

		people = [
			...people,
			{
				id: lastId + 1,
				name: name,
				amount: amount
			}
		];

		name = '';
		amount = 0;
	};

	const remove = (person: any) => {
		people = people.filter((i) => i !== person);
	};

	function calculateDebts() {
		const totalAmount = people.reduce((acc, person) => acc + person.amount, 0);
		const averageAmount = totalAmount / people.length;

		const balances: Record<number, number> = {};

		for (const person of people) {
			const balance = person.amount - averageAmount;
			if (balance !== 0) {
				balances[person.id] = balance;
			}
		}

		while (true) {
			const positiveBalance = Object.entries(balances).find(([_id, balance]) => balance > 0);
			const negativeBalance = Object.entries(balances).find(([_id, balance]) => balance < 0);

			if (!positiveBalance || !negativeBalance) {
				break;
			}

			const [positiveId, positiveBalanceAmount] = positiveBalance;
			const [negativeId, negativeBalanceAmount] = negativeBalance;

			const transferAmount = Math.min(
				Math.abs(positiveBalanceAmount),
				Math.abs(negativeBalanceAmount)
			);
			debts = [
				...debts,
				{ from: Number(negativeId), to: Number(positiveId), amount: transferAmount }
			];

			balances[Number(positiveId)] = positiveBalanceAmount - transferAmount;
			balances[Number(negativeId)] = negativeBalanceAmount + transferAmount;
		}

		document.getElementById('my-modal').checked = true;
	}

	function closeModal() {
		document.getElementById('my-modal').checked = false;
		setTimeout(() => {
			debts = [];
		}, 1000);
	}
</script>

<svelte:head>
	<title>splitty</title>
</svelte:head>

<section class="mt-5">
	<input type="checkbox" id="my-modal" class="modal-toggle" />
	<div class="modal">
		<div class="modal-box">
			<h3 class="font-bold text-lg">these are the results 🥁</h3>
			<ul>
				{#each debts as debt}
					<li>
						{people.find((p) => p.id === debt.from)?.name} owes {Math.round(
							(debt.amount + Number.EPSILON) * 100
						) / 100} to {people.find((p) => p.id === debt.to)?.name}
					</li>
				{/each}
			</ul>
			<div class="modal-action">
				<btn class="btn" on:click={() => closeModal()}>Yay!</btn>
			</div>
		</div>
	</div>

	<div class="flex flex-col md:flex-row mb-5">
		<div class="flex-auto md:w-64">
			<form on:submit|preventDefault={addPerson}>
				<input
					type="text"
					placeholder="name"
					class="input input-bordered max-w-lg"
					bind:value={name}
					required
				/>
				<input
					type="number"
					placeholder="amount"
					class="input input-bordered mt-2 max-w-lg"
					bind:value={amount}
					required
				/>
				<button class="btn w-full mt-2 max-w-lg">add</button>
			</form>
		</div>
	</div>

	<div class="flex-auto mt-2">
		<button
			class="btn w-full md:w-auto"
			on:click={() => calculateDebts()}
			disabled={people.length <= 0}>calculate</button
		>
	</div>

	<div class="flex-auto mt-2">
		<button
			class="btn w-full md:w-auto"
			on:click={() => {
				people = [];
			}}
			disabled={people.length <= 0}>clean</button
		>
	</div>
	<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 mt-2">
		{#each people as person}
			<PersonCard {person} {remove} />
		{/each}
	</div>
</section>
