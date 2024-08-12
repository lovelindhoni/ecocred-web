<script lang="ts">
	import { goto } from '$app/navigation';
	import Speedometer from '$lib/components/Speedometer.svelte';
	import user from '$lib/assets/profile.png';
	import bin from '$lib/assets/ecologism-recycle-svgrepo-com (1).svg';
	import leaves from '$lib/assets/green-leaves-svgrepo-com.svg';
	import logo from '$lib/assets/ecology-sprout-svgrepo-com.svg';
	const jwt = sessionStorage.getItem('jwt');
	if (jwt == null) {
		goto('/');
	}
	const email = sessionStorage.getItem('email');
	let graphData;
	let homeData;
	const home = async () => {
		const response = await fetch('https://eco-cred.vercel.app/api/home/', {
			method: 'GET',
			headers: {
				Authorization: `Bearer ${jwt}`
			}
		});
		if (!response.ok) {
			throw new Error(`HTTP error! status: ${response.status}`);
		}
		const data = await response.json();
		console.log(data);
		homeData = data;
	};
	const graph = async () => {
		const response = await fetch('https://eco-cred.vercel.app/api/graph_analytics/', {
			method: 'GET',
			headers: {
				Authorization: `Bearer ${jwt}`
			}
		});
		if (!response.ok) {
			throw new Error(`HTTP error! status: ${response.status}`);
		}
		const data = await response.json();
		console.log(data);
		graphData = data;
	};
	home();
	graph();
</script>

<main class="box-border flex h-[100vh] w-[100vw] flex-col">
	<div class="flex w-full justify-between bg-[#22c55e] p-8 text-white">
		<div class="flex items-center justify-center gap-4">
			<img src={logo} class="h-20 w-20" alt="" />
			<p class="text-5xl font-bold">EcoCred</p>
		</div>
		<div class="flex items-center gap-4">
			<img src={user} class="h-12 w-12" alt="" />
			<p class="text-xl font-bold">{email}</p>
		</div>
	</div>
	<div class="flex flex-1 flex-col items-center justify-center">
		<div class=" flex w-fit items-center justify-center gap-24 rounded-xl p-8">
			<img src={user} class="h-24 w-24" alt="" />
			<div>
				<p class="text-2xl">{email}</p>
				<p class="text-2xl">East Delhi</p>
			</div>
		</div>
		<div class="flex w-full flex-1 border border-black">
			{#if homeData}
				<div class="flex-2 box-border items-center justify-center border border-black px-12 pt-12">
					<div class="flex h-full flex-col items-center justify-center">
						<Speedometer value={homeData?.average_green_score} />
						<div class="flex items-center">
							<img src={leaves} class="h-24 w-24" alt="" />
							<div class="flex flex-col gap-2 text-xl">
								<p class="font-bold">Average Green Score</p>
								<p>{homeData.average_green_score}</p>
							</div>
						</div>
					</div>
				</div>
				<div class="flex-1 border border-black p-12"></div>
				<div class="flex-2 flex flex-col justify-center border border-black p-12">
					<div class="flex items-center gap-4">
						<img src={bin} class="h-24 w-24" alt="" />
						<div class="flex flex-col gap-2 text-xl">
							<p class="font-bold">Recycle Score</p>
							<p>{homeData.average_recycle_score}</p>
						</div>
					</div>
					<div class="mt-14 flex w-full flex-col justify-center space-y-5">
						<div class="flex space-x-10 text-2xl">
							<p>Electricity Bill linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						<div class="flex items-center space-x-10 text-2xl">
							<p>Water Bill linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						<div class="flex items-center space-x-10 text-2xl">
							<p>LPG linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
					</div>
				</div>
			{/if}
		</div>
	</div>
</main>
