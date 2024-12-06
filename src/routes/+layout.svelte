<script lang="ts">
	import '../app.css';
	import { page } from '$app/stores';
	import * as Sidebar from '$lib/components/ui/sidebar/index.ts';
	import * as Breadcrumb from '$lib/components/ui/breadcrumb/index.ts';
	import { Separator } from '$lib/components/ui/separator/index.ts';
	import AppSidebar from '$lib/components/app-sidebar.svelte';
	let { children } = $props();
</script>

<Sidebar.Provider>
	<AppSidebar />
	<Sidebar.Inset>
		<header class="sticky top-0 flex h-16 shrink-0 items-center gap-2 border-b bg-background px-4">
			<Sidebar.Trigger class="-ml-1" />
			<Separator orientation="vertical" class="mr-2 h-4" />
			<Breadcrumb.Root>
				<Breadcrumb.List>
					<Breadcrumb.Item>
						<Breadcrumb.Page
							>{$page.url.pathname.slice($page.url.pathname.lastIndexOf('/')) == '/'
								? 'Home'
								: $page.url.pathname
										.slice($page.url.pathname.lastIndexOf('/') + 1)[0]
										.toUpperCase() +
									$page.url.pathname.slice(
										$page.url.pathname.lastIndexOf('/') + 2
									)}</Breadcrumb.Page
						>
					</Breadcrumb.Item>
				</Breadcrumb.List>
			</Breadcrumb.Root>
		</header>
		<div class="flex flex-1 flex-col gap-4 p-4">
			{@render children()}
		</div>
	</Sidebar.Inset>
</Sidebar.Provider>
