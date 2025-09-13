<script lang="ts">
	import { cn } from '../utils/index.js';
	import { buttonVariants, type ButtonProps } from './button.svelte.js';

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
