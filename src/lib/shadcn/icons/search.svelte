<script lang="ts">
	import Search from 'lucide-svelte/icons/search';
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface Props {
		class?: ClassValue;
		size?: number;
	}

	let { class: className, size = 16 }: Props = $props();
</script>

<Search class={cn('h-4 w-4', className)} {size} />
