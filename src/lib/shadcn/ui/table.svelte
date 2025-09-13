<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface TableProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: TableProps = $props();
</script>

<div class="relative w-full overflow-auto">
	<table class={cn('w-full caption-bottom text-sm', className)}>
		{@render children()}
	</table>
</div>
