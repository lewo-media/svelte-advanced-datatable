<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface TableHeaderProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: TableHeaderProps = $props();
</script>

<thead class={cn('[&_tr]:border-b', className)}>
	{@render children()}
</thead>
