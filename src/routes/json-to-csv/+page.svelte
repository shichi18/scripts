<script lang="ts">
	import fileSaver from 'file-saver';
	import { json2csv } from 'json-2-csv';

	let jsonData: Array<{ [key: string]: string | number }> = [];

	function handleFileUpload(event: Event) {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files.length > 0) {
			const file = input.files[0];
			const reader = new FileReader();

			reader.onload = async (e) => {
				const text = e.target?.result as string;
				jsonData = JSON.parse(text);
			};

			reader.readAsText(file);
		}
	}

	function downloadCsv() {
		try {
			const csv = json2csv(jsonData);
			const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
			fileSaver.saveAs(blob, 'data.csv');
		} catch (err) {
			console.error(err);
		}
	}
</script>

<main>
	<h1>JSON to CSV Converter</h1>
	<input type="file" accept=".json" on:change={handleFileUpload} />
	<button on:click={downloadCsv} disabled={jsonData.length === 0}>Download CSV</button>
	{#if jsonData.length > 0}
		<h2>JSON Input</h2>
		<div class="pre-code">
			<pre>{JSON.stringify(jsonData, null, 2)}</pre>
		</div>
	{/if}
</main>

<style>
    .pre-code {
        text-align: center;
    }

    .pre-code pre {
        text-align: left;
        display: inline-block;
        white-space: pre-wrap;
        margin: 0 auto;
        padding: 10px;
    }
</style>
