<script>
	import { onMount } from 'svelte';
	import { fade, scale } from 'svelte/transition';

	let days, hours, minutes, seconds;
	let targetDate = new Date('2024-11-02T00:00:00').getTime();

	let quotes = [
		{ text: 'Ты крутая', author: 'Карл Маркс' },
		{
			text: 'Делай сегодня то, что другие не хотят, и завтра будешь там, где другие не могут.',
			author: 'Неизвестен'
		},
		{ text: 'Нет ничего невозможного.', author: 'Аристотель' },
		{
			text: 'Жизнь — это 10% того, что с вами происходит, и 90% того, как вы на это реагируете.',
			author: 'Чарльз Суиндолл'
		}
	];

	let currentQuoteIndex = 0;
	let currentQuote = quotes[currentQuoteIndex];

	// Функция для обновления таймера
	function updateCountdown() {
		const now = new Date().getTime();
		const timeLeft = targetDate - now;

		if (timeLeft > 0) {
			days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
			hours = Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
			minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
			seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);
		} else {
			days = hours = minutes = seconds = 0;
		}
	}

	// Обновляем цитаты каждые 5 секунд
	function updateQuote() {
		currentQuoteIndex = (currentQuoteIndex + 1) % quotes.length;
		currentQuote = quotes[currentQuoteIndex];
	}

	// Используем onMount для установки интервалов
	onMount(() => {
		updateCountdown();
		const countdownInterval = setInterval(updateCountdown, 1000);
		const quoteInterval = setInterval(updateQuote, 5000); // Каждые 5 секунд обновляем цитату

		return () => {
			clearInterval(countdownInterval);
			clearInterval(quoteInterval);
		};
	});
</script>

<main>
	<section class="hero" transition:fade={{ duration: 1000 }}>
		<h1>До твоего дня рождения осталось</h1>
		<div class="countdown">
			<div class="time-box" transition:scale={{ duration: 500 }}>
				<span class="number">{days}</span>
				<span class="label">Дней</span>
			</div>
			<div class="time-box" transition:scale={{ duration: 500 }}>
				<span class="number">{hours}</span>
				<span class="label">Часов</span>
			</div>
			<div class="time-box" transition:scale={{ duration: 500 }}>
				<span class="number">{minutes}</span>
				<span class="label">Минут</span>
			</div>
			<div class="time-box" transition:scale={{ duration: 500 }}>
				<span class="number">{seconds}</span>
				<span class="label">Секунд</span>
			</div>
		</div>
	</section>

	<!-- Блок цитат -->
	<section class="quotes">
		<div class="quote-container quote-display">
			<blockquote transition:fade>
				"{currentQuote.text}"
			</blockquote>
			<p class="author" transition:fade>- {currentQuote.author}</p>
		</div>
	</section>
</main>

<style>
	.quote-display {
		font-family: 'Playfair Display', serif;
		font-optical-sizing: auto;
		font-weight: 400;
		font-style: normal;
	}
	main {
		font-family: 'Arial', sans-serif;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
		background: linear-gradient(120deg, #0f0c29, #302b63, #24243e);
		color: #fff;
		text-align: center;
		overflow: hidden;
	}

	h1 {
		font-size: 3rem;
		margin-bottom: 20px;
	}

	.countdown {
		display: flex;
		justify-content: center;
		gap: 20px;
		margin-bottom: 40px;
	}

	.time-box {
		background: rgba(255, 255, 255, 0.1);
		border-radius: 10px;
		padding: 20px;
		min-width: 100px;
		transition: all 0.3s ease;
	}

	.time-box:hover {
		transform: scale(1.1);
		background: rgba(255, 255, 255, 0.2);
	}

	.number {
		font-size: 2.5rem;
		font-weight: bold;
		display: block;
		margin-bottom: 5px;
	}

	.label {
		font-size: 1rem;
		opacity: 0.8;
	}

	/* Стиль блока с цитатами */
	.quotes {
	}

	.quote-container {
		background-color: rgba(68, 96, 255, 0.1);
		padding: 20px;
		border-radius: 15px;
		width: 500px;
		margin: 0 auto;
		animation: fadeInOut 5s ease infinite;
	}

	blockquote {
		font-size: 1.5rem;
		font-style: italic;
		margin-bottom: 10px;
	}

	.author {
		font-size: 1rem;
		opacity: 0.8;
	}

	/* Анимация для плавной смены цитат */
	@keyframes fadeInOut {
		0% {
			opacity: 0;
		}
		10% {
			opacity: 1;
		}
		90% {
			opacity: 1;
		}
		100% {
			opacity: 0;
		}
	}
</style>
