<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface TableRowProps {
		class?: ClassValue;
		children: any;
		onclick?: () => void;
	}

	let { class: className, children, onclick }: TableRowProps = $props();
</script>

<tr
	class={cn(
		'border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted',
		className
	)}
	{onclick}
>
	{@render children()}
</tr>
