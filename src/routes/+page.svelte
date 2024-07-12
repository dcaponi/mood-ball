<script lang="ts">
	import TacoBellMp3 from '$lib/assets/tacobell.mp3';

	let ballSays = '';
	let emotion = '';

	let Color: 'yellow' | 'blue' | 'red' | 'green' | 'purple' | 'pink';
	let Emotion:
		| 'happy horny'
		| 'sad horny'
		| 'angry horny'
		| 'terror'
		| 'disgust... but horny'
		| 'horny';
	let MoodRingValue: [typeof Color, typeof Emotion];

	let moodRingValues: (typeof MoodRingValue)[] = [
		['yellow', 'happy horny'],
		['blue', 'sad horny'],
		['red', 'angry horny'],
		['green', 'terror'],
		['purple', 'disgust... but horny'],
		['pink', 'horny']
	];

	const EIGHT_BALL_RESPONSES = [
		[
			'🟢 It is certain',
			'🟢 It is decidedly so',
			'🟢 Without a doubt',
			'🟢 Yes definitely',
			'🟢 You may rely on it',
			'🟢 As I see it, yes',
			'🟢 Most likely',
			'🟢 Outlook good',
			'🟢 Yes',
			'🟢 Signs point to yes'
		],
		[
			'🟡 Reply hazy, try again',
			'🟡 Ask again later',
			'🟡 Better not tell you now',
			'🟡 Cannot predict now',
			'🟡 Concentrate and ask again'
		],
		[
			"🔴 Don't count on it",
			'🔴 My reply is no',
			'🔴 My sources say no',
			'🔴 Outlook not so good',
			'🔴 Very doubtful'
		]
	];

	const getRandomNumberBetween = (min: number, max: number) =>
		Math.floor(Math.random() * (max - min) + min);

	const makePrediction = () => {
		const audioPlayer: HTMLAudioElement = document.getElementById(
			'audioPlayer'
		) as HTMLAudioElement;
		if (audioPlayer) {
			audioPlayer.src = TacoBellMp3;
			audioPlayer.play();
		}

		const ballSentimentIndex = getRandomNumberBetween(0, EIGHT_BALL_RESPONSES.length - 1);
		const ballSaysIndex = getRandomNumberBetween(
			0,
			EIGHT_BALL_RESPONSES[ballSentimentIndex].length - 1
		);
		const emotionIndex = getRandomNumberBetween(0, moodRingValues.length - 1);

		ballSays = EIGHT_BALL_RESPONSES[ballSentimentIndex][ballSaysIndex];
		const emotionColor = moodRingValues[emotionIndex][0];
		emotion = moodRingValues[emotionIndex][1];
	};

	let question: string | null = null;
</script>

<body>
	<h1>Mood Ring 8-Ball</h1>
	<p>We didn't hire a designer</p>

	<audio id="audioPlayer"></audio>

	<lable for="question">Ask Forth Your Question:</lable>
	<input type="text" bind:value={question} /><br /><br />
	<button on:click={makePrediction}>Push Me!</button>
	{#if ballSays !== '' && emotion !== ''}
		<h2>The supreme 8-ball brought to you by Taco Bell has decalred {ballSays}.</h2>
		<h3>This fills you with {emotion}</h3>
	{/if}
</body>

<style>
	body {
		margin: 50px;
	}
	h2 {
		text-wrap: wrap;
	}
</style>
