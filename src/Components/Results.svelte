<script>
	// @ts-nocheck

	import { storeInputValue } from '../store.js';
	import { onMount, afterUpdate } from 'svelte';

	/**
	 * @type {{ [x: string]: any; }[] | null}
	 */
	let fetchedData = null;
	let errorFetchingData = false;
	let showScrollButton = false;
	let previousInputValue = '';
	let synonymClicked = false;

	$: {
		if ($storeInputValue) {
			fetchData($storeInputValue);
		}
	}

	onMount(() => {
		checkOverflow();
	});

	afterUpdate(() => {
		checkOverflow();
	});

	function checkOverflow() {
		const scrollContainer = document.getElementById('scrollContainer');
		showScrollButton = scrollContainer.scrollHeight > scrollContainer.clientHeight;
	}

	function scrollToBottom() {
		const scrollContainer = document.getElementById('scrollContainer');
		scrollContainer.scrollTop = scrollContainer.scrollHeight;
	}

	/**
	 * @param {string} value
	 */
	async function fetchData(value) {
		try {
			const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${value}`);
			const jsonData = await response.json();
			fetchedData = jsonData;
			console.log(fetchedData);
			if (!Array.isArray(fetchedData) || fetchedData.length === 0) {
				errorFetchingData = true;
			} else {
				errorFetchingData = false;
			}
		} catch (error) {
			console.error('Error fetching data:', error);
			errorFetchingData = true;
		} finally {
			console.log('Done fetching data');
		}
	}
</script>

<div class="flex flex-col items-center justify-center">
	<div
		class="mt-4 p-4 rounded-md card w-[90%] md:w-[70%] variant-soft-surface max-h-[600px] overflow-y-auto hide-scrollbar"
		id="scrollContainer"
	>
		{#if synonymClicked}
			<button
				on:click={() => {
					synonymClicked = false;
					storeInputValue.set(previousInputValue);
				}}
				class="border border-blue-500 rounded-md px-2 py-1 mr-2 mb-2 bg-blue-500 hover:text-white transition-colors duration-300"
				>{`<`}</button
			>
		{/if}
		{#if fetchedData && !errorFetchingData}
			<main class="p-2">
				{#each fetchedData as item}
					{#each item.meanings as meaning}
						<h3 class="text-lg font-semibold mt-4">{meaning.partOfSpeech}</h3>
						<ol class="list-decimal ml-6">
							{#each meaning.definitions as definition}
								<li class="mt-2">{definition.definition}</li>
								{#if definition.synonyms.length > 0}
									<div class="flex flex-wrap mt-1">
										{#each definition.synonyms as synonym}
											<button
												on:click={() => {
													previousInputValue = $storeInputValue;
													synonymClicked = true;
													storeInputValue.set(synonym);
												}}
												class="border border-blue-500 text-blue-500 rounded-md px-2 py-1 mr-2 mb-2 hover:bg-blue-500 hover:text-white transition-colors duration-300"
											>
												{synonym}
											</button>
										{/each}
									</div>
								{/if}
							{/each}
						</ol>
					{/each}
				{/each}
				{#if showScrollButton}
					<div class="absolute bottom-4 right-4">
						<button
							class="text-2xl ring-1 px-2 py-2 rounded-full shadow transition-colors duration-400"
							on:click={scrollToBottom}
						>
							🔻
						</button>
					</div>
				{/if}
			</main>
		{/if}

		{#if errorFetchingData}
			<div class="flex flex-col items-center justify-center">
				<h2 class="text-2xl font-bold">No results found</h2>
				<p class="text-lg mt-4">Try searching for something else or go back to previous results</p>
				<button
					on:click={() => {
						if (synonymClicked) {
							synonymClicked = false;
							storeInputValue.set(previousInputValue);
						} else {
							storeInputValue.set('Hello');
						}
					}}
					class="border border-blue-500 text-blue-500 rounded-md px-2 py-1 mr-2 mb-2 hover:bg-blue-500 hover:text-white transition-colors duration-300"
					>back</button
				>
			</div>
		{/if}
	</div>
</div>
