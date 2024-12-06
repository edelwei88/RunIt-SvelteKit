<script lang="ts">
	import SquareUser from 'lucide-svelte/icons/square-user';
	import LogOut from 'lucide-svelte/icons/log-out';
	import Key from 'lucide-svelte/icons/key';
	import ChevronsUpDown from 'lucide-svelte/icons/chevrons-up-down';

	import * as Sidebar from '$lib/components/ui/sidebar/index.ts';
	import { useSidebar } from '$lib/components/ui/sidebar/index.ts';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu/index.ts';

	import { token } from '$lib/stores/token.ts';

	function cleartoken() {
		token.reset();
		location.reload();
	}

	let {
		user
	}: {
		user: {
			name: string;
			email: string;
		};
	} = $props();

	const sidebar = useSidebar();
</script>

{#if user.name !== ''}
	<Sidebar.Menu>
		<Sidebar.MenuItem>
			<DropdownMenu.Root>
				<DropdownMenu.Trigger>
					{#snippet child({ props })}
						<Sidebar.MenuButton
							{...props}
							size={sidebar.state == 'collapsed' ? 'sm' : 'lg'}
							class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
						>
							<SquareUser />
							<div class="grid flex-1 text-left text-sm leading-tight">
								<span class="truncate font-semibold">{user.name}</span>
								<span class="truncate text-xs">{user.email}</span>
							</div>
							<ChevronsUpDown class="ml-auto size-4" />
						</Sidebar.MenuButton>
					{/snippet}
				</DropdownMenu.Trigger>
				<DropdownMenu.Content
					class="w-[--bits-dropdown-menu-anchor-width] min-w-56 rounded-lg"
					side={sidebar.isMobile ? 'bottom' : 'right'}
					align="end"
					sideOffset={4}
				>
					<DropdownMenu.Label class="p-0 font-normal">
						<div class="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
							<SquareUser class="h-8 w-8" />
							<div class="grid flex-1 text-left text-sm leading-tight">
								<span class="truncate font-semibold">{user.name}</span>
								<span class="truncate text-xs">{user.email}</span>
							</div>
						</div>
					</DropdownMenu.Label>
					<DropdownMenu.Separator />
					<DropdownMenu.Item onclick={cleartoken}>
						<LogOut />
						Log out
					</DropdownMenu.Item>
				</DropdownMenu.Content>
			</DropdownMenu.Root>
		</Sidebar.MenuItem>
	</Sidebar.Menu>
{:else}
	<Sidebar.Menu>
		<Sidebar.MenuItem>
			<Sidebar.MenuButton>
				{#snippet child({ props })}
					<a href="/login" {...props}><Key /> Login</a>
				{/snippet}
			</Sidebar.MenuButton>
		</Sidebar.MenuItem>
	</Sidebar.Menu>
{/if}
