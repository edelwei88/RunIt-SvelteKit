<script lang="ts">
	import House from 'lucide-svelte/icons/house';
	import Code from 'lucide-svelte/icons/code';

	import * as Sidebar from '$lib/components/ui/sidebar/index.ts';
	import NavUser from '$lib/components/app-sidebar-user.svelte';

	import { get } from 'svelte/store';
	import { token } from '$lib/stores/token.ts';

	const items: any[] = [
		{
			title: 'Home',
			url: '/',
			icon: House
		},
		{
			title: 'Compile',
			url: '/compile',
			icon: Code
		}
	];

	const user = { name: '', email: '' };
	const token_obj: any = { token: get(token) };
	if (get(token) !== '') {
		fetch('http://localhost:3000/getuserbytoken', {
			method: 'POST',
			body: JSON.stringify(token_obj),
			headers: {
				'Content-Type': 'application/json; charset=UTF-8'
			}
		})
			.then((r) => {
				return r.json();
			})
			.then((data) => {
				if (Object.keys(data).length !== 0) {
					user.name = data.username;
					user.email = data.email;
				}
			});
	}
</script>

<Sidebar.Root collapsible="icon">
	<Sidebar.Content>
		<Sidebar.Group>
			<Sidebar.GroupLabel>Compiler</Sidebar.GroupLabel>
			<Sidebar.GroupContent>
				<Sidebar.Menu>
					{#each items as item (item.title)}
						<Sidebar.MenuItem>
							<Sidebar.MenuButton>
								{#snippet child({ props })}
									<a href={item.url} {...props}>
										<item.icon />
										<span>{item.title}</span>
									</a>
								{/snippet}
							</Sidebar.MenuButton>
						</Sidebar.MenuItem>
					{/each}
				</Sidebar.Menu>
			</Sidebar.GroupContent>
		</Sidebar.Group>
	</Sidebar.Content>
	<Sidebar.Footer>
		<NavUser {user} />
	</Sidebar.Footer>
</Sidebar.Root>
