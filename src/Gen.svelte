<!-- Gen.svelte -->

<script>
	// Import qrcode.js
	import QRCode from "qrcode-svg";

	const ECLs = ["L", "M", "H", "Q"];

	let text = "";
	let outputRef;
	let generatedSVG = "";

	let colorDark = "#000000";
	let colorLight = "#ffffff";
	let ecl = "H";

	const generate = async (e) => {
		e.preventDefault();

		const qr = new QRCode({
			content: text,
			padding: 2,
			color: colorDark,
			background: colorLight,
			ecl,
			container: "svg-viewbox",
		});
		const svg = qr.svg();

		generatedSVG = svg;
		outputRef.innerHTML = svg;
	};

	function dataURItoBlob(dataURI) {
		// convert base64/URLEncoded data component to raw binary data held in a string
		var byteString;
		if (dataURI.split(",")[0].indexOf("base64") >= 0)
			byteString = atob(dataURI.split(",")[1]);
		else byteString = unescape(dataURI.split(",")[1]);

		// separate out the mime component
		var mimeString = dataURI.split(",")[0].split(":")[1].split(";")[0];

		// write the bytes of the string to a typed array
		var ia = new Uint8Array(byteString.length);
		for (var i = 0; i < byteString.length; i++) {
			ia[i] = byteString.charCodeAt(i);
		}

		return new Blob([ia], { type: mimeString });
	}

	const shareSupported = !!navigator.share;
	const share = () => {
		// Convert SVG to JPG then share
		let img = new Image();
		let svg = new Blob([generatedSVG], { type: "image/svg+xml" });
		let url = URL.createObjectURL(svg);

		img.onload = () => {
			// Create a canvas and draw the image
			let canvas = document.createElement("canvas");
			canvas.width = img.width;
			canvas.height = img.height;
			let ctx = canvas.getContext("2d");

			ctx.drawImage(img, 0, 0);
			window.URL.revokeObjectURL(url);
			let png = canvas.toDataURL("image/jpeg");
			canvas.remove();

			// Convert to blob
			const blob = dataURItoBlob(png);

			// Share
			navigator.share({
				title: "QR Code",
				files: [new File([blob], "qr.jpeg", { type: "image/jpeg" })],
			});
		};
		img.src = url;
	};
</script>

<div class="output" bind:this={outputRef}>
	<svg height="0" width="0" />
</div>
{#if generatedSVG && shareSupported}
	<button class="secondary" on:click={share}>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			width="16"
			height="16"
			fill="currentColor"
			class="bi bi-share-fill"
			viewBox="0 0 16 16"
		>
			<path
				d="M11 2.5a2.5 2.5 0 1 1 .603 1.628l-6.718 3.12a2.5 2.5 0 0 1 0 1.504l6.718 3.12a2.5 2.5 0 1 1-.488.876l-6.718-3.12a2.5 2.5 0 1 1 0-3.256l6.718-3.12A2.5 2.5 0 0 1 11 2.5"
			/>
		</svg>
		Share
	</button>
{/if}

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
