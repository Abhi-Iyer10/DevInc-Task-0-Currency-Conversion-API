<script>
  import { onMount } from 'svelte';
	
	let currencyMap = new Map();
	let amount;
  let fromCurrency = '';
  let toCurrency = '';
  let result = 0;

  onMount(async () => {
    try {
      const response = await fetch('https://latest.currency-api.pages.dev/v1/currencies/inr.json');
      const data = await response.json();
			if (data.inr) {
	      currencyMap = new Map(Object.entries(data.inr));
	    }

			console.log(currencyMap.get("usd"));
    } catch (error) {
      console.error("Error fetching currency data:", error);
    }
  });

	function convertCurrency() {
    const fromRate = currencyMap.get(fromCurrency);
    const toRate = currencyMap.get(toCurrency);
    if (fromRate && toRate) {
      result = (amount * toRate) / fromRate;
    } else {
      result = "Conversion rate not available";
    }
  }
	
</script>

<div class="input-container">
  <input type="number" bind:value={amount} placeholder="Amount" />
	<br>
  <input type="text" bind:value={fromCurrency} placeholder="From Currency (Lowercase)" />
	<br>
  <input type="text" bind:value={toCurrency} placeholder="To Currency (Lowercase)" />
	<br>
  <button on:click={convertCurrency}>Calculate</button>
</div>

<p>Result: {result}</p>
