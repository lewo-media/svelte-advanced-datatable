<script lang="ts">
	import ChevronsRight from 'lucide-svelte/icons/chevrons-right';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<ChevronsRight class={cn('h-4 w-4', className)} {size} />
