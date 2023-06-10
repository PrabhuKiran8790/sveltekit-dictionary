<script>
	// @ts-nocheck
	import { storeInputValue } from '../store.js';

	let inputValue = '';
	let displayValue = '';

	function handleSubmit() {
		if (inputValue.length > 0) {
			storeInputValue.set(inputValue);
			storeInputValue.subscribe((value) => {
				displayValue = value;
			});
			inputValue = '';
		}
	}
</script>

<div class="card variant-soft-surface flex justify-center mt-5 mx-3 lg:mx-24">
	<div class="w-4/5">
		<div class="container mx-auto py-8">
			<h1 class="h1 text-center">
				<span
					class="bg-gradient-to-br from-blue-500 to-cyan-300 bg-clip-text text-transparent box-decoration-clone"
					>Dictionary</span
				>
			</h1>
			<p class="text-center mt-1 mb-10">A Svelte Project</p>
			<div class="flex items-center justify-center">
				<input
					bind:value={inputValue}
					on:keydown={(e) => {
						if (e.key === 'Enter' && inputValue.length > 0) {
							handleSubmit();
						}
					}}
					type="text"
					placeholder="Search"
					class="text-black font-bold w-64 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				/>
				<button
					on:click={handleSubmit}
					class="ml-4 px-4 py-2 bg-blue-500 text-white font-semibold rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
					>Submit</button
				>
			</div>
		</div>
		{#if displayValue}
			<div class="select-none">
				<p class="text-center">
					Results for: <span class="text-lg font-bold text-teal-300 underline">{displayValue}</span>
				</p>
			</div>
		{/if}
	</div>
</div>
