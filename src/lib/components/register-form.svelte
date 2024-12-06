<script lang="ts">
	import { goto } from '$app/navigation';
	import { Button } from '$lib/components/ui/button/index.ts';
	import * as Card from '$lib/components/ui/card/index.ts';
	import { Input } from '$lib/components/ui/input/index.ts';
	import { Label } from '$lib/components/ui/label/index.ts';

	const url = 'http://localhost:3000/register';
	let user = $state({
		login: '',
		email: '',
		password: ''
	});

	function handle_submit() {
		fetch(url, {
			method: 'POST',
			body: JSON.stringify(user),
			headers: {
				'Content-type': 'application/json; charset=UTF-8'
			}
		}).then((r) => {
			if (r.ok) {
				alert('Success');
				goto('/login');
			} else alert('Username or email is taken');
		});
	}
</script>

<Card.Root class="mx-auto my-auto max-w-sm">
	<Card.Header>
		<Card.Title class="text-2xl">Register</Card.Title>
		<Card.Description>Fill and send the form to register your account</Card.Description>
	</Card.Header>
	<Card.Content>
		<div class="grid gap-4">
			<div class="grid gap-2">
				<Label for="username">Username</Label>
				<Input bind:value={user.login} id="username" type="text" required />
			</div>
			<div class="grid gap-2">
				<Label for="email">Email</Label>
				<Input
					bind:value={user.email}
					id="email"
					type="email"
					placeholder="email@domain.com"
					required
				/>
			</div>
			<div class="grid gap-2">
				<div class="flex items-center">
					<Label for="password">Password</Label>
				</div>
				<Input bind:value={user.password} id="password" type="password" required />
			</div>
			<Button onclick={handle_submit} type="submit" class="w-full">Sign up</Button>
		</div>
	</Card.Content>
</Card.Root>
