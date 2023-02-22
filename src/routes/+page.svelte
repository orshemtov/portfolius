<script>
  import { DonutChart } from "@carbon/charts-svelte"

  import "@carbon/styles/css/styles.css"
  import "@carbon/charts/styles.css"

  const portfolio = {
    withdrawals: [],
    deposits: [],
    holdings: [
      { symbol: "GLD", quantity: 5 },
      { symbol: "VGIT", quantity: 6 },
      { symbol: "SLV", quantity: 11 },
      { symbol: "VOO", quantity: 14 },
      { symbol: "VGLT", quantity: 20 },
    ],
  }

  portfolio.holdings = portfolio.holdings.map((holding) => {
    const price = fetchQuote(holding.symbol)
    return {
      ...holding,
      price,
      value: holding.quantity * price,
    }
  })

  const total = portfolio.holdings.reduce((acc, holding) => acc + holding.value, 0)

  function fetchQuote(symbol) {
    return (Math.random() * 20 + 100)
  }

</script>

<svelte:head>
    <title>Home</title>
    <meta name="description" content="Svelte demo app"/>
</svelte:head>

<section>
    <h1 class="text-4xl">
        Portfolius
    </h1>
    <table class="table-auto border-collapse">
        <thead>
        <tr>
            <th class="p-2 border">Symbol</th>
            <th class="p-2 border">Quantity</th>
            <th class="p-2 border">Price</th>
            <th class="p-2 border">Value</th>
        </tr>
        </thead>
        <tbody>
        {#each portfolio.holdings as holding}
            <tr>
                <td class="p-2 border">{holding.symbol}</td>
                <td class="p-2 border">{holding.quantity}</td>
                <td class="p-2 border">{holding.price.toFixed(2)}</td>
                <td class="p-2 border">{holding.value.toFixed(2)}</td>
            </tr>
        {/each}
        </tbody>
        <tfoot>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td class="p-2 border">{total.toFixed(2)}</td>
        </tr>
        </tfoot>
    </table>

    <section>
        <DonutChart
                data={portfolio.holdings.map((holding) => ({
                group: holding.symbol,
                   value: holding.value,
                }))}
                options={{
	"resizable": true,
	"donut": {
		"center": {
			"label": "$"
		}
	},
	"height": "300px"
}}
        />
    </section>
</section>