<script>
	import Header from '../Components/Header.svelte';
	import Results from '../Components/Results.svelte';
	import { storeInputValue } from '../store.js';

	let inputValue;

	storeInputValue.subscribe((value) => {
		inputValue = value;
	});
</script>

<div class="overflow-x-hidden">
	<Header />
	<Results />
</div>
