<script lang="ts">
	import { goto } from '$app/navigation';
	import Logo from '$lib/components/Logo.svelte';
	import Modal from '$lib/components/Modal.svelte';
	import { isUserLoggedIn, demo } from '$lib/stores/store';
	import ecology from '$lib/assets/ecology-sprout-svgrepo-com.svg';

	import { onMount } from 'svelte';
	const citiesByState = {
		'Andhra Pradesh': [
			'Visakhapatnam',
			'Vijayawada',
			'Guntur',
			'Nellore',
			'Tirupati',
			'Kakinada',
			'Rajahmundry',
			'Kurnool'
		],
		Delhi: ['New Delhi', 'Delhi'],
		Karnataka: [
			'Bengaluru',
			'Mysuru',
			'Hubli',
			'Mangalore',
			'Belgaum',
			'Davangere',
			'Bellary',
			'Bijapur'
		],
		Maharashtra: [
			'Mumbai',
			'Pune',
			'Nagpur',
			'Nashik',
			'Thane',
			'Aurangabad',
			'Solapur',
			'Amravati'
		],
		'Tamil Nadu': [
			'Chennai',
			'Coimbatore',
			'Madurai',
			'Tiruchirappalli',
			'Salem',
			'Tirunelveli',
			'Vellore',
			'Erode'
		],
		'Uttar Pradesh': [
			'Lucknow',
			'Kanpur',
			'Ghaziabad',
			'Agra',
			'Varanasi',
			'Meerut',
			'Allahabad',
			'Bareilly'
		],
		'West Bengal': [
			'Kolkata',
			'Howrah',
			'Durgapur',
			'Siliguri',
			'Asansol',
			'Bardhaman',
			'Kharagpur',
			'Malda'
		]
	};

	let selectedState = '';
	let selectedCity = '';
	let email = '';
	let password = '';
	let elec = 0;
	let lpg = 0;
	let water = 9;
	let username = '';
	const jwt = sessionStorage.getItem('jwt');
	if (jwt !== null) goto('/dashboard');

	// Get the list of states from the keys of the citiesByState object
	const states = Object.keys(citiesByState);

	// Reactive statement to update cities based on the selected state
	$: cities = selectedState ? citiesByState[selectedState] : [];

	function handleStateChange(event) {
		selectedState = event.target.value;
		selectedCity = ''; // Reset city when state changes
	}

	function handleCityChange(event) {
		selectedCity = event.target.value;
	}

	const signup = async (event) => {
		event.preventDefault();
		console.log(username, email, password);
		console.log('api call started');
		const response = await fetch('https://eco-cred.vercel.app/api/create_account/', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				name: username.trim(),
				email: email.trim(),
				new_password: password.trim(),
				electricity_bill_number: elec.toString(),
				lpg_service_number: lpg.toString(),
				water_bill_number: water.toString(),
				city: selectedCity,
				state: selectedState
			})
		});
		const data = await response.json();
		console.log(data);
		window.alert(
			'Unfortunately your city is not supported yet. Please use a demo account in meanwhile'
		);
		$isUserLoggedIn = true;
		my_modal_4.showModal();
	};
	const login = async () => {
		console.log('api call started login');
		const response = await fetch('https://eco-cred.vercel.app/api/sign_in/', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				email: email.trim(),
				password: password.trim()
			})
		});
		const data = await response.json();
		console.log(data.status);
		if (data.status === 'failure') {
			window.alert("Sorry user account doesn't exist, please create a new Accounts");
			$isUserLoggedIn = false;
		}
	};
</script>

<main class="box-border flex h-[100vh] w-[100vw] bg-gradient-to-r from-green-500 to-green-600">
	<div class="box-border w-[50%] p-16">
		<img src={ecology} class="h-48 w-48" alt="" />
		<h1 class="mt-12 text-7xl font-bold text-white">Unlock Sustainable Financing with EcoCred</h1>
		<p class="mt-8 text-xl font-bold text-white">
			ecocred is an initiative that calculates your 'green score' to help you access financial loans
			that reward your eco-friendly lifestyle.
		</p>
	</div>
	<div class="flex w-[50%] items-center justify-center">
		<form
			action=""
			class="scrollable box-border h-[90%] w-[60%] overflow-y-auto rounded-xl bg-white p-12"
		>
			<caption class="flex text-2xl font-bold"
				>{isUserLoggedIn ? 'Log in' : 'Sign up'} to EcoCred</caption
			>
			<Modal />

			{#if $isUserLoggedIn}
				<p class="mt-8 text-xl">Email</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M2.5 3A1.5 1.5 0 0 0 1 4.5v.793c.026.009.051.02.076.032L7.674 8.51c.206.1.446.1.652 0l6.598-3.185A.755.755 0 0 1 15 5.293V4.5A1.5 1.5 0 0 0 13.5 3h-11Z"
						/>
						<path
							d="M15 6.954 8.978 9.86a2.25 2.25 0 0 1-1.956 0L1 6.954V11.5A1.5 1.5 0 0 0 2.5 13h11a1.5 1.5 0 0 0 1.5-1.5V6.954Z"
						/>
					</svg>
					<input type="email" placeholder="Email" class="grow" required bind:value={email} />
				</label>
				<p class="mt-8 text-xl">Password</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							fill-rule="evenodd"
							d="M14 6a4 4 0 0 1-4.899 3.899l-1.955 1.955a.5.5 0 0 1-.353.146H5v1.5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1-.5-.5v-2.293a.5.5 0 0 1 .146-.353l3.955-3.955A4 4 0 1 1 14 6Zm-4-2a.75.75 0 0 0 0 1.5.5.5 0 0 1 .5.5.75.75 0 0 0 1.5 0 2 2 0 0 0-2-2Z"
							clip-rule="evenodd"
						/>
					</svg>
					<input type="password" class="grow" required bind:value={password} />
				</label>
				<button
					class="btn btn-lg btn-wide mt-12 bg-[#22c55e] text-2xl font-bold text-white hover:bg-[hsl(142,71%,65%)]"
					on:click={(event) => {
						event.preventDefault();
						login();
					}}>Log In</button
				>
				<p class="mt-12 text-xl">In a hurry? Have a quick</p>
				<button
					class="btn btn-lg btn-wide mt-4 bg-[#22c55e] text-2xl font-bold text-white hover:bg-[hsl(142,71%,65%)]"
					on:click={() => my_modal_4.showModal()}>Demo</button
				>
			{:else}
				<p class="mt-8 text-xl">Email</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M2.5 3A1.5 1.5 0 0 0 1 4.5v.793c.026.009.051.02.076.032L7.674 8.51c.206.1.446.1.652 0l6.598-3.185A.755.755 0 0 1 15 5.293V4.5A1.5 1.5 0 0 0 13.5 3h-11Z"
						/>
						<path
							d="M15 6.954 8.978 9.86a2.25 2.25 0 0 1-1.956 0L1 6.954V11.5A1.5 1.5 0 0 0 2.5 13h11a1.5 1.5 0 0 0 1.5-1.5V6.954Z"
						/>
					</svg>
					<input type="email" placeholder="Email" class="grow" required bind:value={email} />
				</label>
				<p class="mt-8 text-xl">Password</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							fill-rule="evenodd"
							d="M14 6a4 4 0 0 1-4.899 3.899l-1.955 1.955a.5.5 0 0 1-.353.146H5v1.5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1-.5-.5v-2.293a.5.5 0 0 1 .146-.353l3.955-3.955A4 4 0 1 1 14 6Zm-4-2a.75.75 0 0 0 0 1.5.5.5 0 0 1 .5.5.75.75 0 0 0 1.5 0 2 2 0 0 0-2-2Z"
							clip-rule="evenodd"
						/>
					</svg>
					<input type="password" class="grow" required bind:value={password} />
				</label>
				<p class="mt-8 text-xl">Username</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z"
						/>
					</svg>
					<input type="text" class="grow" placeholder="Username" bind:value={username} />
				</label>
				<p class="mt-8 text-xl">Electricity Bill number</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z"
						/>
					</svg>
					<input
						type="number"
						class="grow"
						bind:value={elec}
						required
						placeholder="Electricity Bill number"
					/>
				</label>
				<p class="mt-8 text-xl">LPG service number</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z"
						/>
					</svg>
					<input
						type="number"
						class="grow"
						bind:value={lpg}
						required
						placeholder="LPG service number"
					/>
				</label>
				<p class="mt-8 text-xl">Water Bill number</p>
				<label class="input input-bordered mt-2 flex items-center gap-2">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 16 16"
						fill="currentColor"
						class="h-4 w-4 opacity-70"
					>
						<path
							d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z"
						/>
					</svg>
					<input
						type="number"
						class="grow"
						bind:value={water}
						required
						placeholder="Water Bill number"
					/>
				</label>
				<p for="state-select" class="mt-8 text-xl">State</p>
				<select
					id="state-select"
					class="select select-bordered mt-2 w-full"
					required
					on:change={handleStateChange}
				>
					<option value="">Select a state</option>
					{#each states as state}
						<option value={state}>{state}</option>
					{/each}
				</select>

				<!-- City Selection -->
				<p for="city-select" class="mt-8 text-xl">City</p>
				<select
					id="city-select"
					class="select select-bordered mt-2 w-full"
					bind:value={selectedCity}
					on:change={handleCityChange}
					disabled={!selectedState}
					required
				>
					<option value="">Select a city</option>
					{#each cities as city}
						<option value={city}>{city}</option>
					{/each}
				</select>
				<button
					class="btn btn-lg btn-wide mt-12 bg-[#22c55e] text-2xl font-bold text-white hover:bg-[hsl(142,71%,65%)]"
					on:click={signup}>Sign Up</button
				>
			{/if}
		</form>
	</div>
</main>
