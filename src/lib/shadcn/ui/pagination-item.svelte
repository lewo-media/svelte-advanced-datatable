<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface PaginationItemProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: PaginationItemProps = $props();
</script>

<li class={cn('', className)}>
	{@render children()}
</li>
