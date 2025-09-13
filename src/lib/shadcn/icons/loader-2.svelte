<script lang="ts">
	import Loader2 from 'lucide-svelte/icons/loader-2';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<Loader2 class={cn('h-4 w-4 animate-spin', className)} {size} />
