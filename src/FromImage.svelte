<!-- Scan.svelte -->

<script>
	import QrScanner from "qr-scanner";

	let fileRef;
	let output = "";
	let processing = false;

	const scanFromFile = async () => {
		const file = fileRef.files[0];
		processing = true;
		try {
			const result = await QrScanner.scanImage(file, {
				returnDetailedScanResult: true,
			});
			output = result.data;
		} catch (e) {
			output = "<Failed to scan image>";
		}
		processing = false;
	};
</script>

<label for="qr">QR code image</label>
<input type="file" accept="image/*" bind:this={fileRef} />
<button on:click={scanFromFile}> Scan </button>

{#if processing}
	<progress />
{:else}
	<pre>{output}</pre>
{/if}

<style>
	button {
		width: 100%;
		margin-bottom: 1rem;
	}

	pre {
		padding: 1rem;
	}
</style>
