<script lang="ts">
	import { goto } from '$app/navigation';
	import Speedometer from '$lib/components/Speedometer.svelte';
	import user from '$lib/assets/profile.png';
	import bin from '$lib/assets/ecologism-recycle-svgrepo-com (1).svg';
	import leaves from '$lib/assets/green-leaves-svgrepo-com.svg';
	import logo from '$lib/assets/ecology-sprout-svgrepo-com.svg';
	import Graph from '$lib/components/Graph.svelte';
	const jwt = sessionStorage.getItem('jwt');
	if (jwt == null) {
		goto('/');
	}

	const email = sessionStorage.getItem('email');

	const logout = () => {
		sessionStorage.clear();
		goto('/');
	};
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
		console.log("home", data);
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
		return data;
	};
	home();
</script>

<main class="box-border flex h-[100vh] w-[100vw] flex-col">
	<div class="navbar h-28 bg-[#22c55e]">
		<div class="flex w-full items-center justify-between mx-4 ">
			<div class="flex items-center justify-center space-x-5 cursor-pointer" on:click={() => goto('/dashboard')}>
				<img src={logo} class="h-12 w-12" alt="" />
				<p class="text-3xl font-bold text-white">EcoCred</p>
			</div>
			<div class="flex space-x-10 text-2xl font-semibold text-white">
				<div class="underline cursor-pointer">Dashboard</div>
				<div on:click={() => goto('/chat')} class="cursor-pointer">Chat</div>
				<div on:click={() => goto('/about-us')} class="cursor-pointer">About Us</div>
			</div>
			<div class="flex items-center justify-center gap-2">
				{#if homeData}
					<p class="text-2xl text-white font-semibold cursor-pointer">{homeData.user_data.Name}</p>
				{/if}

				<div class="dropdown dropdown-end">
					<div tabindex="0" role="button" class="avatar btn btn-circle btn-ghost">
						<div class="w-10 rounded-full">
							<img alt="Tailwind CSS Navbar component" src={user} />
						</div>
					</div>
					<ul
						tabindex="0"
						class="menu border dropdown-content menu-sm z-[1] mt-3 w-fit rounded-box bg-base-100 p-2 shadow"
					>
						<li>
							<a href="#" class="justify-between">
								{email}
							</a>
						</li>
						<li on:click={logout}><a>Logout</a></li>
					</ul>
				</div>
			</div>
		</div>
	</div>
	<div class="flex flex-1 flex-col items-center justify-center">
		<div class="flex w-full flex-1 border border-[#ccc]">
			{#if homeData}
				<div
					class="flex-2 box-border items-center justify-center border border-[#ccc] px-12 pt-[3vh]"
				>
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
				<div class="my-auto flex-1 p-[2vw]">
					{#await graph()}
						<p>...</p>
					{:then data}
						<Graph rawData={data.data} />
					{/await}
				</div>
				<div class="flex-2 pn flex flex-col justify-center border border-[#ccc] p-12">
					<div class="flex items-center gap-4">
						<img src={bin} class="h-24 w-24" alt="" />
						<div class="flex flex-col gap-2 text-xl">
							<p class="font-bold">Recycle Score</p>
							<p>{homeData.average_recycle_score}</p>
						</div>
					</div>
					<div class="mt-14 flex w-full flex-col items-center justify-center space-y-5">
						{#if homeData.iot_available}
						<div class="flex w-full items-center justify-between space-x-10 text-2xl">
							<p>IOT Product ID linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						{:else}
						<div class="flex w-full items-center justify-between space-x-10 text-2xl">
							<p>Electricity Bill linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						<div class="flex w-full items-center justify-between space-x-10 text-2xl">
							<p>Water Bill linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						<div class="flex w-full items-center justify-between space-x-10 text-2xl">
							<p>LPG linked</p>
							<span class="badge bg-[#22c55e] p-4 text-2xl font-bold text-white">Success</span>
						</div>
						{/if}
					</div>
				</div>
			{/if}
		</div>
	</div>
</main>
