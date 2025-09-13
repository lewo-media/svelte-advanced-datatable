<script lang="ts">
	import ChevronsLeft from 'lucide-svelte/icons/chevrons-left';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<ChevronsLeft class={cn('h-4 w-4', className)} {size} />
