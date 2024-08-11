<script lang="ts">
	import { goto } from '$app/navigation';
	import Speedometer from '$lib/components/Speedometer.svelte';
    import user from "$lib/assets/user-circle-svgrepo-com.svg"
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

<main class="box-border h-[100vh] w-[100vw] flex flex-col">
	<div class="flex w-full justify-between bg-[#22c55e] p-8 text-white">
		<p class="text-4xl">Ecocred</p>
		<div class="flex gap-4 items-center">
			<img src={user} class="h-12 w-12" alt="" />
			<p class="text-xl">{email}</p>
		</div>
	</div>
	<div class="flex-1 flex flex-col items-center justify-center">
		<div class="border border-black w-fit flex p-8 rounded-xl justify-center items-center gap-8">
			<img src={user} class="h-24 w-24" alt="" />
            <div>
			<p>{email}</p>
			<p>East Delhi</p>
            </div>
		</div>
        <div class="flex-1 flex border border-black w-full ">
		{#if homeData}                                
        <div class="border border-black flex-1 p-12 flex">
			<Speedometer value={homeData?.average_green_score} />
            <p class="text-2xl">Average Green Score: <br>{homeData.average_green_score}</p>
        </div>
        <div class="border border-black flex-1 p-12">
			<Speedometer value={homeData?.average_green_score} />
        </div>
        <div class="border border-black flex-1 p-12">
			<Speedometer value={homeData?.average_green_score} />
        </div>
		{/if}
        </div>
	</div>
</main>
