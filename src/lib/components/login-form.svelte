<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.ts';
	import * as Card from '$lib/components/ui/card/index.ts';
	import { Input } from '$lib/components/ui/input/index.ts';
	import { Label } from '$lib/components/ui/label/index.ts';
	import { get } from 'svelte/store';
	import { token } from '$lib/stores/token.ts';
	import { goto } from '$app/navigation';

	const url = 'http://localhost:3000/login';
	let user = $state({
		login: '',
		password: ''
	});

	function handle_submit() {
		fetch(url, {
			method: 'POST',
			body: JSON.stringify(user),
			headers: {
				'Content-type': 'application/json; charset=UTF-8'
			}
		})
			.then((r) => {
				if (r.ok) return r.json();
				else alert('Wrong password or username');
			})
			.then((data) => {
				if (data !== undefined) {
					token.set(data.token);
					window.location.href = '/';
				}
			});
	}
	console.log(get(token));
</script>

<Card.Root class="mx-auto my-auto max-w-sm">
	<Card.Header>
		<Card.Title class="text-2xl">Login</Card.Title>
		<Card.Description>Enter your username below to login to your account</Card.Description>
	</Card.Header>
	<Card.Content>
		<div class="grid gap-4">
			<div class="grid gap-2">
				<Label for="username">Username</Label>
				<Input bind:value={user.login} id="username" type="text" required />
			</div>
			<div class="grid gap-2">
				<div class="flex items-center">
					<Label for="password">Password</Label>
				</div>
				<Input bind:value={user.password} id="password" type="password" required />
			</div>
			<Button type="submit" class="w-full" onclick={handle_submit}>Login</Button>
		</div>
		<div class="mt-4 text-center text-sm">
			Don't have an account?
			<a href="/register" class="underline"> Sign up </a>
		</div>
	</Card.Content>
</Card.Root>
