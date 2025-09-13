<script lang="ts">
	import { ComponentType } from '$lib/dataComponent/ComponentType.js';
	import type { FullDataTableConfig } from '$lib/types/DataTableConfig.js';
	import { getConfigContext, getDataSourceContext } from '$lib/util/context.js';
	import type { ClassValue } from 'svelte/elements';
	import { fade } from 'svelte/transition';
	import TableRow from './ui/table-row.svelte';
	import TableCell from './ui/table-cell.svelte';
	import Checkbox from './ui/checkbox.svelte';
	import DropdownMenuTrigger from './ui/dropdown-menu-trigger.svelte';
	import { cn } from './utils';

	interface Props {
		item: any;
		index: number;
		config: FullDataTableConfig<any>;
		openIndex: number | undefined;
		open: (index: number) => void;
		onClick?: (item: any) => void;
		highlighted?: boolean;
		striped?: boolean;
		hoverable?: boolean;
		selected?: boolean;
		onToggleSelection?: (item: any) => void;
	}

	let {
		item,
		index,
		config,
		openIndex,
		open,
		onClick,
		highlighted = false,
		striped = false,
		hoverable = true,
		selected = false,
		onToggleSelection
	}: Props = $props();
	let dataSource = $derived(getDataSourceContext());

	let isOpen = $derived(openIndex === index);

	let rowClass = $derived(
		cn(
			{
				'bg-muted/50': highlighted,
				'bg-muted/25': striped && index % 2 === 1,
				'hover:bg-muted/50': hoverable && !highlighted
			}
		)
	);
</script>

<TableRow class={rowClass} onclick={() => onClick?.(item)}>
	<TableCell class="w-12">
		<Checkbox checked={selected} onclick={() => onToggleSelection?.(item)} />
	</TableCell>
	{#each Object.entries(config.columnProperties) as [key, colProp] (key)}
		{#if !colProp.hidden}
			<TableCell class={cn(key === 'actions' && "w-12")}>
				{#if colProp.componentType === ComponentType.CUSTOM}
					<svelte:component this={colProp.component} {item} {index} />
				{:else if colProp.componentType === ComponentType.BOOLEAN}
					<div class="flex items-center justify-center">
						<div
							class={cn(
								'h-2 w-2 rounded-full',
								item[colProp.propertyKey] ? 'bg-green-500' : 'bg-gray-300'
							)}
						></div>
					</div>
				{:else if colProp.componentType === ComponentType.DATE}
					{@const date = new Date(item[colProp.propertyKey])}
					{@const formattedDate = date.toLocaleDateString(colProp.locale, colProp.formatOptions)}
					<span class="text-sm">{formattedDate}</span>
				{:else if colProp.componentType === ComponentType.NUMBER}
					{@const formattedNumber = new Intl.NumberFormat(colProp.locale, colProp.formatOptions).format(
						item[colProp.propertyKey]
					)}
					<span class="text-sm font-mono">{formattedNumber}</span>
				{:else if colProp.componentType === ComponentType.STRING}
					<span class="text-sm">{item[colProp.propertyKey]}</span>
				{:else if colProp.componentType === ComponentType.ENUM}
					{@const enumValue = colProp.enumValues.find((enumItem) => enumItem.value === item[colProp.propertyKey])}
					{#if enumValue}
						<span class="text-sm">{enumValue.label}</span>
					{:else}
						<span class="text-sm text-muted-foreground">Unknown</span>
					{/if}
				{:else}
					<span class="text-sm">{item[colProp.propertyKey]}</span>
				{/if}
			</TableCell>
		{/if}
	{/each}
	<TableCell class="w-12">
		<DropdownMenuTrigger />
	</TableCell>
</TableRow>

{#if isOpen && config.expandableRows}
	<TableRow>
		<TableCell colspan={Object.keys(config.columnProperties).length}>
			<div in:fade|local={{ duration: 200 }} out:fade|local={{ duration: 200 }}>
				{#if config.expandableRowComponent}
					<svelte:component this={config.expandableRowComponent} {item} {index} />
				{:else}
					<div class="p-4">
						<pre class="text-sm">{JSON.stringify(item, null, 2)}</pre>
					</div>
				{/if}
			</div>
		</TableCell>
	</TableRow>
{/if}
