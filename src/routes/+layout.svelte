<script>
	import '../app.css';

	import Navbar from './Navbar.svelte';
</script>

<div class="app px-5 mx-auto max-w-[1536px]">
	<Navbar />

	<main>
		<slot />
	</main>

	<footer />
</div>
