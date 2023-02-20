<script lang="ts">
	import type { Debt, Person } from './interfaces';

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

		document.getElementById('name-input')?.focus();
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
	<!-- modal -->
	<input type="checkbox" id="my-modal" class="modal-toggle" />
	<div class="modal">
		<div class="modal-box">
			{#if debts.length > 0}
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
			{:else}
				<h3 class="font-bold text-lg">no debts to calculate 🎉</h3>
			{/if}
			<div class="modal-action">
				<btn class="btn" on:click={() => closeModal()}>Yay!</btn>
			</div>
		</div>
	</div>

	<!-- actions -->
	<div class="mt-10 sm:mt-0">
		<div class="mt-5 md:col-span-2 md:mt-0">
			<form on:submit|preventDefault={addPerson}>
				<div class="overflow-hidden sm:rounded-md">
					<div>
						<div class="grid gap-3 md:grid-cols-12 sm:grid-cols-1">
							<div class="md:col-span-3 sm:col-span-1">
								<input
									type="text"
									placeholder="name"
									class="input input-bordered block w-full"
									bind:value={name}
									id="name-input"
									required
								/>
							</div>

							<div class="md:col-span-3 sm:col-span-1">
								<input
									type="number"
									placeholder="amount"
									class="input input-bordered block w-full"
									bind:value={amount}
									required
								/>
							</div>

							<div class="md:col-span-2 sm:col-span-1">
								<button class="btn w-full" type="submit">add</button>
							</div>
							<div class="md:col-span-2 sm:col-span-1">
								<button
									class="btn w-full"
									on:click={() => calculateDebts()}
									disabled={people.length <= 0}>calculate</button
								>
							</div>
							<div class="md:col-span-2 sm:col-span-1">
								<button
									class="btn w-full"
									on:click={() => {
										people = [];
									}}
									disabled={people.length <= 0}>clean</button
								>
							</div>
						</div>
					</div>
				</div>
			</form>
		</div>
	</div>

	<!-- data -->
	{#if people.length > 0}
		<div class="flex mt-4 overflow-x-auto">
			<table class="table w-full">
				<!-- head -->
				<thead>
					<tr>
						<th>name</th>
						<th>amount</th>
						<th />
					</tr>
				</thead>
				<tbody>
					{#each people as person}
						<tr class="hover">
							<td>{person.name}</td>
							<td>{person.amount}</td>
							<td
								><button class="btn btn-square btn-sm" on:click={() => remove(person)}>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
										><path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M6 18L18 6M6 6l12 12"
										/></svg
									></button
								></td
							>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	{/if}
</section>
