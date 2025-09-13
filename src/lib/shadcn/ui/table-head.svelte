<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface TableHeadProps {
		class?: ClassValue;
		children: any;
		onclick?: () => void;
	}

	let { class: className, children, onclick }: TableHeadProps = $props();
</script>

<th
	class={cn(
		'h-12 px-4 text-left align-middle font-medium text-muted-foreground [&:has([role=checkbox])]:pr-0',
		className
	)}
	{onclick}
>
	{@render children()}
</th>
