<script lang="ts">
	import logo from '$lib/assets/ecology-sprout-svgrepo-com.svg';
	import { goto } from '$app/navigation';
	import realistic from '$lib/assets/realistic-nature.jpeg';
	import animated from '$lib/assets/animated-nature.jpeg';
</script>

<main class="box-border flex h-[100dvh] w-[100dvw] flex-col">
	<div class="navbar h-[13%] bg-[#22c55e]">
		<div class="mx-4 flex w-full items-center justify-between">
			<div
				class="flex cursor-pointer items-center justify-center space-x-5"
				on:click={() => goto('/dashboard')}
			>
				<img src={logo} class="h-12 w-12" alt="" />
				<p class="text-3xl font-bold text-white">EcoCred</p>
			</div>
			<div class="flex space-x-10 text-2xl font-semibold text-white">
				<div on:click={() => goto('/dashboard')} class="cursor-pointer">Dashboard</div>
				<div on:click={() => goto('/chat')} class="cursor-pointer">Chat</div>
				<div class="cursor-pointer underline">About Us</div>
			</div>
		</div>
	</div>

	<div class="box-border flex h-[87%] w-full items-center gap-32 px-32 py-28">
		<img src={realistic} class="h-full w-full rounded-xl" alt="" />
		<div>
			<h1 class="mb-6 text-[8vh] font-bold">About Us</h1>
			<article class="loose text-2xl font-semibold">
				EcoCred is an innovative platform that calculates your Green Score, reflecting your
				commitment to a sustainable lifestyle. By combining this score with your CIBIL rating, we
				help you unlock a range of government benefits, including discounts on medical bills,
				increased pensions, and improved loan options with lower interest rates. Using advanced
				techniques like web scraping and IoT devices, EcoCred empowers you to live more sustainably
				while enjoying tangible rewards.
			</article>
		</div>
	</div>
</main>
