<script lang="ts">
	import type { Person } from './interfaces';

	export let person: Person;
	export let remove: any;

	const onRemove = (personToRemove: Person) => {
		remove(personToRemove);
	};
</script>

<div class="card p-4 bg-neutral text-neutral-content">
	<div class="card-body">
		<div class="flex card-subtitle justify-evenly">
			<div>
				<span>{person.name}</span>
				<span>{person.amount}</span>
			</div>
			<div>
				<button class="btn btn-square btn-sm" on:click={() => onRemove(person)}>
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
					>
				</button>
			</div>
		</div>
	</div>
</div>
