<script lang="ts">
	import ChevronRight from 'lucide-svelte/icons/chevron-right';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<ChevronRight class={cn('h-4 w-4', className)} {size} />
