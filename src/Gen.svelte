<!-- Gen.svelte -->

<script>
	// Import qrcode.js
	import QRCode from "qrcode-svg";

	const ECLs = ["L", "M", "H", "Q"];

	let text = "";
	let outputRef;

	let colorDark = "#000000";
	let colorLight = "#ffffff";
	let ecl = "H";

	const generate = async (e) => {
		e.preventDefault();

		const qrSVG = new QRCode({
			content: text,
			padding: 2,
			color: colorDark,
			background: colorLight,
			ecl,
			container: "svg-viewbox",
		}).svg();

		outputRef.innerHTML = qrSVG;
	};
</script>

<div class="output" bind:this={outputRef}>
	<svg height="0" width="0" />
</div>

<label for="text">Text</label>
<textarea id="text" name="text" bind:value={text} />
<button on:click={generate}>Generate</button>

<div class="colors">
	<label>
		Foreground
		<input type="color" bind:value={colorDark} />
	</label>
	<label>
		Background
		<input type="color" bind:value={colorLight} />
	</label>
</div>

<label>
	Error correction level
	<select bind:value={ecl}>
		{#each ECLs as e}
			<option value={e}>{e}</option>
		{/each}
	</select>
</label>

<style>
	button {
		width: 100%;
	}
	.output {
		margin: 1rem;
	}

	.colors {
		display: flex;
		justify-content: space-between;
		gap: 1rem;
	}
	.colors > label {
		flex: 1;
	}
</style>
