<script>
	import { onMount } from 'svelte';
	import { fade, scale } from 'svelte/transition';
	import gsap from 'gsap';
	import Typed from 'typed.js';
	let password = ''; // Переменная для хранения введенного пароля
	let correctPassword = '936101'; // Задайте правильный пароль
	$: showSecretMessage = false; // Переменная для отображения секретного сообщения
	let message = 'Показать';
	let error = false;

	function revealMessage() {
		console.log(typeof password);
		console.log(password);
		console.log(typeof correctPassword);
		if (password == correctPassword) {
			showSecretMessage = true;
			error = false;
			password = '';
		} else {
			showSecretMessage = false;
			error = true;
			password = '';
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
		const quoteInterval = setInterval(updateQuote, 7000); // Каждые 7 секунд обновляем цитату

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

	let isFocused = false;
	const onFocus = () => (isFocused = true);
	const onBlur = () => (isFocused = false);
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
		<div class="inputs">
			<div class="inputGroup">
				<!-- <label class={isFocused === true ? 'label focusedLabel' : 'label'}>Пароль</label> -->
				<input
					type="password"
					bind:value={password}
					placeholder="Введи пароль"
					class="inputLogin"
					on:focus={onFocus}
					on:blur={onBlur}
				/>
				{#if error}
					<p class="error">Пароль неверный!</p>
				{/if}
			</div>
			<button class="btn reveal-btn" on:click={revealMessage}>Показать</button>
		</div>
	</section>
	<section class="mt-30">
		{#if showSecretMessage}
			<div class="secret-message">
				<p class="typed">С днем рождения, любимая!!!</p>
				<p>❤️</p>
			</div>
			<div class="video">
				<script
					src="https://play.boomstream.com/OV2pfoge/config.jsonp?title=0&color=transparent"
					async
				></script>
				<script
					src="https://play.boomstream.com/assets/javascripts/biframesdk.js?v=1.0.5"
					async
				></script>
				<span
					data-boomstream-code="OV2pfoge"
					data-boomstream-mode="adaptive"
					data-boomstream-use-fullscreen-mode="0"
				/>
			</div>
		{/if}
	</section>
	<!-- <div class="marquee-container">
		<div class="marquee">
			<span
				>До твоего дня рождения осталось всего-то {days} дня! До твоего дня рождения осталось всего-то
				{days} дня! До твоего дня рождения осталось всего-то {days} дня! До твоего дня рождения осталось
				всего-то {days} дня! До твоего дня рождения осталось всего-то {days} дня! До твоего дня рождения
				осталось всего-то {days} дня!
			</span>
		</div>
	</div> -->
</main>

<style>
	.marquee-container {
		overflow: hidden;
		width: 100%;
		background-color: rgba(255, 255, 255, 0.2);
		padding: 10px 0;
		position: fixed;
		bottom: 0;
		left: 0;
	}

	/* Стиль самой бегущей строки */
	.marquee {
		display: inline-block;
		white-space: nowrap;
		animation: marquee 10s linear infinite;
	}

	/* Текст бегущей строки */
	.marquee span {
		font-size: 1.5rem;
		color: #ff69b4;
	}

	/* Ключевые кадры для анимации */
	@keyframes marquee {
		0% {
			transform: translateX(100%);
		}
		100% {
			transform: translateX(-100%);
		}
	}
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

		.focusedLabel {
			color: #302b63;
		}

		.inputGroup input:focus {
			border: 1px solid #302b63 !important;
			outline: none;
		}
	}
	.btn {
		background: var(--text-violet);
		border-radius: 50px;
		color: var(--text-white);
		font-size: 18px;
		margin-top: 10px;
		border: none;
		padding: 19px;
		width: 100%;
		line-height: 22px;
	}

	.btn:hover {
		background: #5d5fef;
		transition: 0.6s;
		cursor: pointer;
	}

	.inputs {
		margin-top: 35px;
	}
	.inputGroup {
		display: flex;
		flex-direction: column;
		color: var(--text-regular);
		position: relative;
	}

	.inputGroup input {
		display: flex;
		padding: 18px 30px;
		border: 1px solid #e4e4e9;
		box-sizing: border-box;
		border-radius: 6px;
	}

	.inputGroup input:focus {
		border: 1px solid var(--text-violet) !important;
		outline: none;
	}
	.error {
		color: rgba(255, 61, 22, 0.914);
		text-align: left;
	}
	.video {
		width: 100%;
		border-radius: 33px;
		margin-top: 30px;
	}
</style>
