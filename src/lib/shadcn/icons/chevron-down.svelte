<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface ChevronDownProps {
		class?: ClassValue;
	}

	let { class: className }: ChevronDownProps = $props();
</script>

<svg
	xmlns="http://www.w3.org/2000/svg"
	width="24"
	height="24"
	viewBox="0 0 24 24"
	fill="none"
	stroke="currentColor"
	stroke-width="2"
	stroke-linecap="round"
	stroke-linejoin="round"
	class={cn('h-4 w-4', className)}
>
	<path d="m6 9 6 6 6-6" />
</svg>
