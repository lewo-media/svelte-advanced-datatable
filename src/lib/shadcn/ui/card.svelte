<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';

	interface CardProps {
		class?: ClassValue;
		children: any;
	}

	let { class: className, children }: CardProps = $props();
</script>

<div class={cn('rounded-lg border bg-card text-card-foreground shadow-sm', className)}>
	{@render children()}
</div>
