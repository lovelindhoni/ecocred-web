<script lang="ts">
	import logo from '$lib/assets/ecology-sprout-svgrepo-com.svg';
	import user from '$lib/assets/profile.png';
	import { HfInference } from '@huggingface/inference';
	import { PUBLIC_HF_TOKEN } from '$env/static/public';
	import { goto } from '$app/navigation';
	import markdownit from 'markdown-it';

	const md = markdownit();

	let app = false;
	let exchanges = [
		{
			owner: 'bot',
			text: 'Hello there, I am Eco Chat, I can answer your queries related to green score'
		}
	];

	const inference = new HfInference(PUBLIC_HF_TOKEN);

	const init = async () => {
		console.log('initing');
		const response = await inference.chatCompletion({
			model: 'mistralai/Mixtral-8x7B-Instruct-v0.1',
			messages: [
				{
					role: 'user',
					content:
						"Just analyze this: My green score is based on my electricity, water, and LPG consumption. Don't respond now. But I will be talking about it from now"
				}
			],
			max_tokens: 500
		});
		const reply = response.choices[0]?.message?.content || '';
		console.log(reply);
		app = true;
	};
	let userText = '';
	$: disabled = !app;
	const sendMessage = async () => {
		userText = userText.trim();
		disabled = true;
		if (userText.length < 2) {
			alert('Please write a longer message for the chatbot to get better results');
			return;
		}
		exchanges = [...exchanges, { owner: 'user', text: userText }];
		exchanges = [
			...exchanges,
			{ owner: 'bot', text: `<span class="loading loading-dots loading-xs"></span>` }
		];
		const response = await inference.chatCompletion({
			model: 'mistralai/Mixtral-8x7B-Instruct-v0.1',
			messages: [{ role: 'user', content: userText }],
			max_tokens: 500
		});
		const reply = response.choices[0]?.message?.content || '';

		exchanges.pop();
		exchanges = exchanges;
		exchanges = [...exchanges, { owner: 'bot', text: reply }];
		disabled = false;
		userText = '';
	};
	const handleKeyUp = (event: KeyboardEvent) => {
		if (event.key === 'Enter') sendMessage();
	};

	const logout = () => {
		sessionStorage.clear();
		goto('/');
	};
</script>

<main class="box-border flex h-[100dvh] w-[100dvw] flex-col">
	<div class="navbar h-28 bg-[#22c55e]">
		<div class="mx-4 flex w-full items-center justify-between">
			<div class="flex items-center justify-center space-x-5 cursor-pointer" on:click={() => goto('/dashboard')}>
				<img src={logo} class="h-12 w-12" alt="" />
				<p class="text-3xl font-bold text-white" >EcoCred</p>
			</div>
			<div class="flex space-x-10 text-2xl font-semibold text-white">
				<div on:click={() => goto('/dashboard')} class="cursor-pointer">Dashboard</div>
				<div class="cursor-pointer underline">Chat</div>
				<div on:click={() => goto('/aboutus')} class="cursor-pointer">About Us</div>
			</div>
		</div>
	</div>
	<div class="mx-auto box-border flex h-[85%] w-1/2 flex-col pt-8">
		<h1 class="text-center text-4xl font-bold">Welcome to EcoChat</h1>
		<div class="scrollable mt-4 box-border flex-1 overflow-y-auto px-8">
			{#await init()}
			<div class="chat chat-start">
				<div class="avatar chat-image">
					<div class="w-10 rounded-full">
						<img
							alt="Tailwind CSS chat bubble component"
							src="https://media.wired.com/photos/592d06185fd65b767a6f2dfa/master/w_2560%2Cc_limit/chat_bot-01-green.jpg"
						/>
					</div>
				</div>
				<div class="chat-bubble box-border flex items-center justify-center p-4">
					{@html `<span class="loading loading-dots loading-xs"></span>`}
				</div>
			</div>
			{:then}
				{#each exchanges as exchange}
					{#if exchange.owner === 'user'}
						<div class="chat chat-end">
							<div class="avatar chat-image">
								<div class="w-10 rounded-full">
									<img alt="Tailwind CSS chat bubble component" src={user} />
								</div>
							</div>
							<div class="chat-bubble box-border flex items-center justify-center p-4">
								{@html exchange.text}
							</div>
						</div>
					{:else}
						<div class="chat chat-start">
							<div class="avatar chat-image">
								<div class="w-10 rounded-full">
									<img
										alt="Tailwind CSS chat bubble component"
										src="https://media.wired.com/photos/592d06185fd65b767a6f2dfa/master/w_2560%2Cc_limit/chat_bot-01-green.jpg"
									/>
								</div>
							</div>
							<div class="chat-bubble box-border flex items-center justify-center p-4">
								{@html exchange.text}
							</div>
						</div>
					{/if}
				{/each}
			{:catch error}
				<p>{JSON.stringify(error)}</p>
			{/await}
		</div>
		<label class="input input-bordered my-4 flex items-center gap-2">
			<input
				type="text"
				class="grow"
				placeholder="Message EcoChat"
				bind:value={userText}
				{disabled}
				on:keyup={handleKeyUp}
			/>
			<svg
				viewBox="0 0 24 24"
				fill="none"
				class="h-5 w-5 opacity-70"
				xmlns="http://www.w3.org/2000/svg"
				on:click={sendMessage}
				role="button"
				on:keyup={sendMessage}
				tabindex="0"
			>
				<path
					d="M11.5003 12H5.41872M5.24634 12.7972L4.24158 15.7986C3.69128 17.4424 3.41613 18.2643 3.61359 18.7704C3.78506 19.21 4.15335 19.5432 4.6078 19.6701C5.13111 19.8161 5.92151 19.4604 7.50231 18.7491L17.6367 14.1886C19.1797 13.4942 19.9512 13.1471 20.1896 12.6648C20.3968 12.2458 20.3968 11.7541 20.1896 11.3351C19.9512 10.8529 19.1797 10.5057 17.6367 9.81135L7.48483 5.24303C5.90879 4.53382 5.12078 4.17921 4.59799 4.32468C4.14397 4.45101 3.77572 4.78336 3.60365 5.22209C3.40551 5.72728 3.67772 6.54741 4.22215 8.18767L5.24829 11.2793C5.34179 11.561 5.38855 11.7019 5.407 11.8459C5.42338 11.9738 5.42321 12.1032 5.40651 12.231C5.38768 12.375 5.34057 12.5157 5.24634 12.7972Z"
					stroke="#000000"
					stroke-width="2"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</label>
	</div>
</main>
