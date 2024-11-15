<script>
	import { page } from '$app/stores';
	import ListItem from '$lib/components/ListItem.svelte';
	let { data } = $props();
	let currentQuestion = $state(0);
	let selectedAnswer = $state('');
	let score = $state(0);
	let progressPercentage = $state(10);
	let submitted = $state(false);

	function handleSubmit() {
		if (selectedAnswer === data.questions[currentQuestion].answer) {
			score++;
		}
		progressPercentage = ((currentQuestion + 1) / data.questions.length) * 100;
	}

	function nextQuestion() {
		currentQuestion++;
	}
</script>

<progress class="progress progress-primary w-56" value={progressPercentage} max="100"></progress>

{#each data.questions as question, index (question.question)}
	{#if index === currentQuestion}
		<p>Question {index + 1} of {data.questions.length}</p>
		<h2>{question.question}</h2>
		{#each question.options as option, index (option)}
			<ListItem title={option} {index} focusAction={() => (selectedAnswer = option)}></ListItem>
		{/each}
	{/if}
{/each}

{#if !submitted}
	<button class="btn btn-primary" onclick={handleSubmit}>Submit</button>
{:else}
	<button class="btn btn-primary" onclick={nextQuestion}>Next Question</button>
{/if}
