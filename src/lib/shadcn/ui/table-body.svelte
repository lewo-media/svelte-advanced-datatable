<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface TableBodyProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: TableBodyProps = $props();
</script>

<tbody class={cn('[&_tr:last-child]:border-0', className)}>
	{@render children()}
</tbody>
