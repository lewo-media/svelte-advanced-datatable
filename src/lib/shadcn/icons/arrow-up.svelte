<script lang="ts">
	import ArrowUp from 'lucide-svelte/icons/arrow-up';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<ArrowUp class={cn('h-4 w-4', className)} {size} />
