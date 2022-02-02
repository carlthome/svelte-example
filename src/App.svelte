<script lang="ts">
	export let name: string;

	import Sub from "./Sub.svelte";

	import * as tf from "@tensorflow/tfjs";

	import * as Tone from 'tone';

	// Define a model for linear regression.
	function train() {
		const model = tf.sequential();
		console.log(model);

		model.add(tf.layers.dense({ units: 1, inputShape: [1] }));

		// Prepare the model for training: Specify the loss and the optimizer.
		model.compile({ loss: "meanSquaredError", optimizer: "sgd" });

		// Generate some synthetic data for training.
		const xs = tf.tensor2d([1, 2, 3, 4], [4, 1]);
		const ys = tf.tensor2d([1, 3, 5, 7], [4, 1]);

		// Train the model using the data.
		model.fit(xs, ys).then(() => {
			// Use the model to do inference on a data point the model hasn't seen before:
			model.predict(tf.tensor2d([5], [1, 1])).print();
		});
	}

	async function play() {
		await Tone.start();
		//create a synth and connect it to the main output (your speakers)
		const synth = new Tone.Synth().toDestination();

		//play a middle 'C' for the duration of an 8th note
		synth.triggerAttackRelease("C4", "8n");
	}
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>
		Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
		how to build Svelte apps.
	</p>
	<button on:click={train}>Train</button>
	<button on:click={play}>Play</button>
	<Sub />
</main>

<canvas>test</canvas>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
