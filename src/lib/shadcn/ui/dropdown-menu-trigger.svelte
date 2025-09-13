<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';
	import MoreHorizontal from '../icons/more-horizontal.svelte';

	interface DropdownMenuTriggerProps {
		class?: ClassValue;
		onclick?: () => void;
	}

	let { class: className, onclick }: DropdownMenuTriggerProps = $props();
</script>

<button
	class={cn(
		'flex h-8 w-8 items-center justify-center rounded-md border border-transparent hover:bg-accent hover:text-accent-foreground',
		className
	)}
	{onclick}
>
	<MoreHorizontal class="h-4 w-4" />
</button>
