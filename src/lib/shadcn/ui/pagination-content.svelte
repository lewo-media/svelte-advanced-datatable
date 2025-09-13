<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface PaginationContentProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: PaginationContentProps = $props();
</script>

<ul class={cn('flex flex-row items-center gap-1', className)}>
	{@render children()}
</ul>
