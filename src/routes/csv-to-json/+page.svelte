<script lang="ts">
	import csv from 'csvtojson';
	import fileSaver from 'file-saver';

	let csvData: Array<{ [key: string]: string | number }> = [];
	let headers: string[] = [];

	function handleFileUpload(event: Event) {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files.length > 0) {
			const file = input.files[0];
			const reader = new FileReader();

			reader.onload = async (e) => {
				const text = e.target?.result as string;
				csvData = await csv().fromString(text);
				if (csvData.length > 0) {
					headers = Object.keys(csvData[0]);
				}
			};

			reader.readAsText(file);
		}
	}

	function downloadJson() {
		const blob = new Blob([JSON.stringify(csvData, null, 2)], { type: 'application/json' });
		fileSaver.saveAs(blob, 'data.json');
	}

</script>

<main>
	<h1>CSV to JSON Converter</h1>
	<input type="file" accept=".csv" on:change={handleFileUpload} />
	<button on:click={downloadJson} disabled={csvData.length === 0}>Download JSON</button>
	{#if csvData.length > 0}
		<h2>CSV Output</h2>
		<table>
			<thead>
			<tr>
				{#each headers as header}
					<th>{header}</th>
				{/each}
			</tr>
			</thead>
			<tbody>
			{#each csvData as row}
				<tr>
					{#each headers as header}
						<td>{row[header]}</td>
					{/each}
				</tr>
			{/each}
			</tbody>
		</table>
	{/if}
</main>

<style>
    table {
        border-collapse: collapse;
				margin: auto;
    }

    th, td {

        border: 1px solid #ddd;
        padding: 8px;
    }
    th {
        background-color: #cb2828;
        text-align: left;
    }
</style>
