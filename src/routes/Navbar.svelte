<script>
	import { editionMode } from '../store';
</script>

<navbar>
	<div class="navbar bg-base-100 p-6 md:p-8">
		<div class="navbar-start">
			<a class="btn btn-ghost normal-case text-xl">dashboard 🎨</a>
		</div>
		<div class="navbar-center">
			{#if $editionMode}
				<a class="btn btn-ghost normal-case text-xl">EDITION MODE</a>
			{/if}
		</div>
		<div class="navbar-end">
			<!-- edition mode -->
			<label class="btn btn-ghost btn-circle swap swap-flip text-2xl mr-5">
				<!-- this hidden checkbox controls the state -->
				<input type="checkbox" />

				<div
					class="swap-off tooltip normal-case"
					data-tip="turn on edition mode"
					on:click={() => {
						editionMode.set(true);
					}}
				>
					💪
				</div>
				<div
					class="swap-on tooltip normal-case"
					data-tip="turn off edition mode"
					on:click={() => {
						editionMode.set(false);
					}}
				>
					😴
				</div>
			</label>

			<!-- notifications -->
			<div class="dropdown dropdown-end mr-5">
				<label tabindex="0" class="btn btn-ghost btn-circle text-2xl">
					<div class="indicator">
						🔔
						<span class="badge badge-sm indicator-item">8</span>
					</div>
				</label>
				<ul
					tabindex="0"
					class="menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-300 rounded-box w-52 shadow"
				>
					<li><a>a notification 😀</a></li>
					<li><a>another notification 🍳</a></li>
					<li><a>last one 💎</a></li>
				</ul>
			</div>

			<!-- profile -->
			<div class="dropdown dropdown-end">
				<label tabindex="0" class="btn btn-ghost btn-circle avatar">
					<div class="w-10 rounded-full">
						<img src="https://placeimg.com/80/80/people" />
					</div>
				</label>
				<ul
					tabindex="0"
					class="menu menu-compact dropdown-content mt-3 p-2 shadow bg-base-300 rounded-box w-52 shadow"
				>
					<li><a>connections 🔌</a></li>
					<li><a>profile 🧑</a></li>
					<li><a>settings ⚙️</a></li>
					<li><a>logout 🚪</a></li>
				</ul>
			</div>
		</div>
	</div>
</navbar>
