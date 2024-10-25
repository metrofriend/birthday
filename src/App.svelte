<script>
	import { onMount } from 'svelte';
	import { fade, scale } from 'svelte/transition';
	import gsap from 'gsap';
	import Typed from 'typed.js';
	let password = ''; // Переменная для хранения введенного пароля
	let correctPassword = '021105'; // Задайте правильный пароль
	$: showSecretMessage = false; // Переменная для отображения секретного сообщения
	let message = 'Показать';

	function revealMessage() {
		console.log(typeof password);
		console.log(password);
		console.log(typeof correctPassword);
		if (password == correctPassword) {
			showSecretMessage = true;
		} else {
			showSecretMessage = false;
			alert('Неправильный пароль!');
		}
	}

	let days, hours, minutes, seconds;
	let targetDate = new Date('2024-11-02T00:00:00').getTime();

	let mesage = 'Нажми :)';
	let parol = true;

	let quotes = [
		{ text: 'Жимеш)', author: 'ты' },
		{
			text: 'Люблю, когда пахнет как мужчина',
			author: 'ты'
		},
		{ text: 'бэгремка.', author: 'я)' },
		{
			text: 'блин, я проспал пару!!!',
			author: 'Ну зато выспался (C) Камила'
		},
		{
			text: 'ТЫ ПОЕЛ?',
			author: '(C) Камила'
		},
		{
			text: 'А?',
			author: '(C) Камила'
		},
		{
			text: 'менты нам не кенты',
			author: '(C) Камила'
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

	//

	let k = 0;

	// Сообщение для Typed.js
	let secretMessage =
		'Ты самая замечательная! Спасибо тебе за твою поддержку, заботу! А еще я хочу сказать, что... [to be continued]';

	// function revealMessage() {
	// 	showSecretMessage = true;
	// 	k += 1;
	// 	if (k == 3) {
	// 		showSecretMessage = false;
	// 		k = 0;
	// 	}
	// 	// Анимация появления текста с помощью Typed.js
	// 	const options = {
	// 		strings: [secretMessage],
	// 		typeSpeed: 40,
	// 		showCursor: false
	// 	};

	// 	new Typed('.typed', options);

	// 	// Анимация блока с сообщением с GSAP
	// 	gsap.from('.secret-message', {
	// 		opacity: 0,
	// 		y: 50,
	// 		duration: 1,
	// 		ease: 'power3.out'
	// 	});
	// }

	onMount(() => {
		gsap.from('.reveal-btn', { opacity: 0, duration: 1, delay: 0.5 });
	});
</script>

<main>
	<section class="hero" transition:fade={{ duration: 1000 }}>
		<h1>Камила, до Вашего дня рождения осталось</h1>
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

	<section class="mt-50">
		<h2>Введите пароль:</h2>
		<!-- Поле ввода для пароля -->
		<input type="password" placeholder="Введите пароль" bind:value={password} />
		<!-- Кнопка для показа сообщения -->
		<button class="reveal-btn" on:click={revealMessage}>
			{message}
		</button>

		<!-- Секретное сообщение будет отображаться только если showSecretMessage true -->
		{#if showSecretMessage}
			<div class="secret-message">
				<p class="typed">Секретное сообщение раскрыто!</p>
				<p>❤️</p>
			</div>
		{/if}
	</section>
</main>

<style>
	.mt-50 {
		margin-top: 50px;
	}
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
		background: linear-gradient(120deg, #0f0c29, #302b63, #24243e);
		min-height: 100vh;
		color: #fff;
		text-align: center;
		overflow: hidden;
		padding: 20px; /* Padding for small screens */
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

	.reveal-btn {
		padding: 10px 20px;
		font-size: 1.2rem;
		background-color: #193753;
		color: #fff;
		border: none;
		cursor: pointer;
		border-radius: 5px;
		margin-top: 20px;
	}

	.reveal-btn:hover {
		background-color: #191342;
	}

	.secret-message {
		max-width: 650px !important;
		margin-top: 20px;
		padding: 20px;
		background: rgba(255, 255, 255, 0.1);
		border-radius: 10px;
		font-size: 1.5rem;
		color: white;
	}

	.typed {
		font-weight: bold;
		color: #ffffff;
		font-family: 'Playfair Display', serif;
		font-optical-sizing: auto;
		font-weight: 400;
		font-style: normal;
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

	/* Адаптивные стили для мобильных устройств */
	@media (max-width: 768px) {
		h1 {
			font-size: 2.5rem;
		}

		.countdown {
			flex-direction: column;
			gap: 15px;
		}

		.time-box {
			min-width: 80px;
			padding: 15px;
		}

		.number {
			font-size: 2rem;
		}

		.label {
			font-size: 0.8rem;
		}

		.quote-container {
			width: 90%;
		}

		blockquote {
			font-size: 1.2rem;
		}

		.secret-message {
			font-size: 1.2rem;
			padding: 15px;
		}

		.reveal-btn {
			font-size: 1rem;
			padding: 8px 15px;
		}
	}

	/* Дополнительная адаптация для очень маленьких экранов */
	@media (max-width: 480px) {
		h1 {
			font-size: 2rem;
		}

		.countdown {
			gap: 10px;
		}

		.time-box {
			min-width: 70px;
			padding: 10px;
		}

		.number {
			font-size: 1.8rem;
		}

		.label {
			font-size: 0.7rem;
		}

		.quote-container {
			width: 100%;
			padding: 10px;
		}

		blockquote {
			font-size: 1rem;
		}

		.secret-message {
			font-size: 1rem;
			padding: 10px;
		}

		.reveal-btn {
			font-size: 0.9rem;
			padding: 7px 12px;
		}

		.inputGroup {
			display: flex;
			flex-direction: column;
			position: relative;
		}

		.inputGroup input {
			display: flex;
			padding: 18px 30px;
			border: 1px solid #e4e4e9;
			box-sizing: border-box;
			border-radius: 6px;
		}

		.label {
			font-size: 14px;
			text-align: left;
			text-transform: uppercase;
			padding: 4px 10px;
			left: 20px;
			top: -12px;
			position: absolute;
			background: #fff;
			align-items: flex-start;
			display: flex;
		}

		.focusedLabel {
			color: #302b63;
		}

		.inputGroup input:focus {
			border: 1px solid #302b63 !important;
			outline: none;
		}
	}
</style>
