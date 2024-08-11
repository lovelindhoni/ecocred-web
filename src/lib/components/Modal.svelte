<script lang="ts">
	import { goto } from '$app/navigation';
	const login = async (email, password) => {
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
		console.log(data);
		sessionStorage.setItem('jwt', data.access_token);
        sessionStorage.setItem("email", email);
		goto('/dashboard');
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
					on:click={() => login('suresh@gmail.com', 'suresh@123')}>Try this</button
				>
			</div>
			<div class="flex w-full items-center justify-between text-xl font-bold">
				<div>riya@gmail.com</div>
				<button
					class="btn bg-[#22c55e] text-white hover:bg-[#22c55e]"
					on:click={() => login('riya@gmail.com', 'riya@123')}>Try this</button
				>
			</div>
			<div class="flex w-full items-center justify-between text-xl font-bold">
				<div>rajat@gmail.com</div>
				<button
					class="btn bg-[#22c55e] text-white hover:bg-[#22c55e]"
					on:click={() => login('rajat@gmail.com', 'rajat@123')}>Try this</button
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
