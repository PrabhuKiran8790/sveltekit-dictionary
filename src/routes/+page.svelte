<script>
// @ts-nocheck

	// @ts-ignore
	import Header from '../lib/components/Header.svelte';
	// @ts-ignore
	import Results from '../lib/components/Results.svelte';
	import { storeInputValue } from '../lib/store.js';


	let inputValue;

	storeInputValue.subscribe((value) => {
		inputValue = value;
	});

</script>

<div class="overflow-x-hidden">
	<Header />
	<Results />
</div>
