<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface PaginationLinkProps {
		class?: ClassValue;
		children: any;
		onclick?: () => void;
		disabled?: boolean;
		isActive?: boolean;
		'aria-label'?: string;
	}

	let { class: className, children, onclick, disabled = false, isActive = false, 'aria-label': ariaLabel }: PaginationLinkProps = $props();
</script>

<button
	class={cn(
		'inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
		isActive
			? 'bg-primary text-primary-foreground hover:bg-primary/90'
			: 'hover:bg-accent hover:text-accent-foreground',
		'h-10 w-10',
		className
	)}
	{onclick}
	{disabled}
	aria-label={ariaLabel}
>
	{@render children()}
</button>
