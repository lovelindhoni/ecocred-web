<script lang="ts">
	import { goto } from '$app/navigation';
	let loading = true;
	let globEmail = 'riya@gmail.com';
	const login = async (event, email, password) => {
		console.time("Shreehari's API");
		const button = event.currentTarget as EventTarget;
		button.innerHTML = `<span class="loading loading-spinner loading-md"></span>`;

		globEmail = email;
		loading = true;
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
		console.timeEnd("Shreehari's API");
		console.log(data);
		sessionStorage.setItem('jwt', data.access_token);
		sessionStorage.setItem('email', email);
		loading = false;
		goto('/dashboard');
	};

	const handleClick = (event: MouseEvent) => {
		const button = event.currentTarget as EventTarget;
		button.innerHTML = `<span class="loading loading-spinner loading-md"></span>`; // Replace the button's content
	};
</script>

<dialog id="my_modal_4" class="modal">
	<div class="max-w-1/2 modal-box">
		<h1 class="text-3xl font-bold">Try some Demo Accounts</h1>
		<div class="mt-12 flex flex-col gap-4">
			<div class="flex w-full items-center justify-between text-xl font-bold">
				<div>suresh@gmail.com</div>
				<button
					class="btn bg-[#22c55e] text-white hover:bg-[#22c55e]"
					on:click={(event) => login(event, 'suresh@gmail.com', 'suresh@123')}>Try this</button
				>
			</div>
			<div class="flex w-full items-center justify-between text-xl font-bold">
				<div>riya@gmail.com</div>
				<button
					class="btn bg-[#22c55e] text-white hover:bg-[#22c55e]"
					on:click={(event) => login(event, 'riya@gmail.com', 'riya@123')}
				>
					Try this</button
				>
			</div>
			<div class="flex w-full items-center justify-between text-xl font-bold">
				<div>rajat@gmail.com</div>
				<button
					class="btn bg-[#22c55e] text-white hover:bg-[#22c55e]"
					on:click={(event) => login(event, 'rajat@gmail.com', 'rajat@123')}>Try this</button
				>
			</div>
		</div>

		<div class="modal-action">
			<form method="dialog">
				<button class="btn btn-circle btn-ghost btn-sm absolute right-2 top-7">✕</button>
			</form>
		</div>
	</div>
</dialog>
