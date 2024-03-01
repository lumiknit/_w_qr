<!-- Scan.svelte -->

<script>
	import QrScanner from "qr-scanner";
    import { onDestroy, onMount } from "svelte";

	let videoRef;
	let outputRef;

	let qrScanner;

	onMount(() => {
		qrScanner = new QrScanner(
			videoRef,
			(result) => {
				outputRef.textContent = result.data;
			},
			{}
		);
		qrScanner.start();
	});

	onDestroy(() => {
		if(qrScanner) {
			qrScanner.stop();
		}
	});
</script>

<video bind:this={videoRef}
/>

<pre bind:this={outputRef}></pre>

<style>
	video {
		width: 100%;
		margin-bottom: 1rem;
	}

	pre {
		padding: 1rem;
	}
</style>
