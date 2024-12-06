<script lang="ts">
	import { Textarea, type TextareaEvents } from '$lib/components/ui/textarea/index.ts';
	import { Button } from '$lib/components/ui/button/index.ts';

	let code: string = $state('');
	let result: string = $state('');

	const url = 'http://localhost:3000/api/compile';

	function compile() {
		fetch(url, {
			method: 'POST',
			body: code,
			headers: {
				'Content-Type': 'text/plain'
			}
		})
			.then((r) => r.text())
			.then((r) => (result = r));
	}
</script>

<div class="m-4 flex h-full w-full items-center justify-center">
	<div class="m-4 flex h-full w-full flex-col">
		<span class="mb-2">Enter your code</span>
		<Textarea bind:value={code} class="h-full resize-none text-lg" spellcheck="false" />
	</div>
	<div class="m-4 flex h-full w-full flex-col overflow-auto break-words">
		<span class="mb-2">Result (stdout or stderr)</span>
		<div class="mb-4 h-3/4 w-full rounded border border-input p-2">
			<span class="break-words text-lg">
				{result}
			</span>
		</div>
		<Button onclick={compile} class="self-start">Compile</Button>
	</div>
</div>
