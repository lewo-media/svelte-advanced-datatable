<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';
	import ChevronDown from '../icons/chevron-down.svelte';

	interface SelectProps {
		value?: string;
		class?: ClassValue;
		onchange?: (event: Event) => void;
		children: any;
	}

	let { value, class: className, onchange, children }: SelectProps = $props();
</script>

<div class="relative">
	<select
		class={cn(
			'flex h-10 w-full items-center justify-between rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background placeholder:text-muted-foreground focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 [&>option]:bg-background [&>option]:text-foreground',
			className
		)}
		{value}
		{onchange}
	>
		{@render children()}
	</select>
	<ChevronDown class="absolute right-3 top-3 h-4 w-4 opacity-50 pointer-events-none" />
</div>
