<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface PaginationProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: PaginationProps = $props();
</script>

<nav role="navigation" aria-label="pagination" class={cn('mx-auto flex w-full justify-center', className)}>
	<ul class="flex flex-row items-center gap-1">
		{@render children()}
	</ul>
</nav>
