<script>
// @ts-nocheck

	// @ts-ignore
	import Header from '../Components/Header.svelte';
	// @ts-ignore
	import Results from '../components/Results.svelte';
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
