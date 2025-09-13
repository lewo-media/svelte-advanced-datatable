<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface TableCellProps {
		class?: ClassValue;
		children: any;
		colspan?: number;
	}

	let { class: className, children, colspan }: TableCellProps = $props();
</script>

<td class={cn('p-4 align-middle [&:has([role=checkbox])]:pr-0', className)} {colspan}>
	{@render children()}
</td>
