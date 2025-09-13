<script lang="ts">
	import ArrowDown from 'lucide-svelte/icons/arrow-down';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<ArrowDown class={cn('h-4 w-4', className)} {size} />
