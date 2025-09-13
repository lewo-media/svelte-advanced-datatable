<script lang="ts">
	import { cn } from '../utils/index.js';
	import type { ClassValue } from 'clsx';
	import CheckIcon from '../icons/check.svelte';

	interface CheckboxProps {
		checked?: boolean;
		disabled?: boolean;
		class?: ClassValue;
		onclick?: () => void;
	}

	let { checked = false, disabled = false, class: className, onclick }: CheckboxProps = $props();
</script>

<button
	type="button"
	role="checkbox"
	aria-checked={checked}
	disabled={disabled}
	class={cn(
		'peer h-4 w-4 shrink-0 rounded-sm border border-primary ring-offset-background focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-primary data-[state=checked]:text-primary-foreground',
		className
	)}
	data-state={checked ? 'checked' : 'unchecked'}
	{onclick}
>
	<CheckIcon class="h-4 w-4 opacity-0 peer-data-[state=checked]:opacity-100" />
</button>
