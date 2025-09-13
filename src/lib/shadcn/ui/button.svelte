<script lang="ts">
	import { cn } from '../utils';
	import { buttonVariants, type ButtonProps } from './button.svelte.ts';

	let {
		class: className,
		variant,
		size,
		disabled = false,
		type = 'button',
		onclick,
		children,
		...restProps
	}: ButtonProps & { children: any } = $props();
</script>

<button
	class={cn(buttonVariants({ variant, size, class: className }))}
	{disabled}
	{type}
	{onclick}
	{...restProps}
>
	{@render children()}
</button>
