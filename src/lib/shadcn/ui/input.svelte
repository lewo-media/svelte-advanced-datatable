<script lang="ts">
	import { cn } from '../utils';
	import type { ClassValue } from 'clsx';

	interface InputProps {
		class?: ClassValue;
		type?: string;
		placeholder?: string;
		value?: string;
		disabled?: boolean;
		oninput?: (event: Event) => void;
		onchange?: (event: Event) => void;
	}

	let {
		class: className,
		type = 'text',
		placeholder,
		value = $bindable(),
		disabled = false,
		oninput,
		onchange,
		...restProps
	}: InputProps = $props();
</script>

<input
	class={cn(
		'flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50',
		className
	)}
	{type}
	{placeholder}
	bind:value
	{disabled}
	{oninput}
	{onchange}
	{...restProps}
/>
