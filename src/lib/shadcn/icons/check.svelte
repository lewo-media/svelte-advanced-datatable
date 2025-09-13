<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface CheckProps {
		class?: ClassValue;
	}

	let { class: className }: CheckProps = $props();
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
	<polyline points="20,6 9,17 4,12" />
</svg>
