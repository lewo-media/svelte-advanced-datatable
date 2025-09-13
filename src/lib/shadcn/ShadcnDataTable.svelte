<script lang="ts">
	import type { DataTableIcon } from '$lib/daisyUi/daisyUiWrappedComponentPropertyMap.js';
	import type { IDataSource } from '$lib/dataSource/IDataSource.js';
	import DataTable from '$lib/internal/index.js';
	import type { ParsedSearchQuery } from '$lib/searchParser/ParsedSearchQuery.js';
	import type { DataTableConfig, FullDataTableConfig } from '$lib/types/DataTableConfig.js';
	import type { MessageFormatter } from '$lib/types/MessageFormatter.js';
	import { setConfigContext, setDataSourceContext, setMessageFormatterContext } from '$lib/util/context.js';
	import { mergeDataTableConfigDefaults } from '$lib/util/dataTableConfigUtil.js';
	import { clamp } from '$lib/util/generalUtil.js';
	import { createMessageFormatter } from '$lib/util/messageFormatterUtil.svelte.js';
	import type { Component, Snippet } from 'svelte';
	import type { ClassValue } from 'svelte/elements';
	import { fade } from 'svelte/transition';
	
	// Shadcn components
	import Table from './ui/table.svelte';
	import TableHeader from './ui/table-header.svelte';
	import TableBody from './ui/table-body.svelte';
	import TableRow from './ui/table-row.svelte';
	import TableHead from './ui/table-head.svelte';
	import TableCell from './ui/table-cell.svelte';
	import Input from './ui/input.svelte';
	import Card from './ui/card.svelte';
	import Checkbox from './ui/checkbox.svelte';
	import Select from './ui/select.svelte';
	import DropdownMenuTrigger from './ui/dropdown-menu-trigger.svelte';
	import Search from './icons/search.svelte';
	import Loader2 from './icons/loader-2.svelte';
	import ArrowUpDown from './icons/arrow-up-down.svelte';
	import ArrowUp from './icons/arrow-up.svelte';
	import ArrowDown from './icons/arrow-down.svelte';
	import ShadcnDataTablePagination from './ShadcnDataTablePagination.svelte';
	import ShadcnDataRow from './ShadcnDataRow.svelte';
	import { cn } from './utils';

	interface Props {
		config: DataTableConfig<any>;

		/**
		 * The data source where the dataTable requests the table data from
		 */
		dataSource: IDataSource<any>;

		icons?: Partial<Record<DataTableIcon, Component>>;

		class?: ClassValue;
		striped?: boolean;
		hoverable?: boolean;

		headerFirst?: Snippet;
		headerAfterSearch?: Snippet;
		headerMiddle?: Snippet;
	}

	let {
		config: configExport,
		dataSource = $bindable(),
		striped = false,
		hoverable = true,
		class: classExport,
		headerFirst,
		headerAfterSearch,
		headerMiddle
	}: Props = $props();

	let config: FullDataTableConfig<unknown> = $derived(mergeDataTableConfigDefaults<unknown>(configExport));
	let format: MessageFormatter = $derived(createMessageFormatter<unknown>(config));

	setConfigContext(() => config);
	setDataSourceContext(() => dataSource);
	setMessageFormatterContext(() => format);

	let currentPage = $state(1);
	let searchInput = $state('');
	let searchQuery = $state<ParsedSearchQuery | undefined>(undefined);
	let selectedRows = $state<Set<any>>(new Set());
	let selectAll = $state(false);

	function toggleSelectAll() {
		if (selectAll) {
			selectedRows.clear();
		} else {
			items.forEach(item => selectedRows.add(item[config.dataUniquePropertyKey]));
		}
		selectAll = !selectAll;
	}

	function toggleRowSelection(item: any) {
		const key = item[config.dataUniquePropertyKey];
		if (selectedRows.has(key)) {
			selectedRows.delete(key);
		} else {
			selectedRows.add(key);
		}
		// Update select all state
		selectAll = selectedRows.size === items.length;
	}

	function isRowSelected(item: any): boolean {
		return selectedRows.has(item[config.dataUniquePropertyKey]);
	}
</script>

<DataTable.Root {searchQuery} {currentPage}>
	{#snippet children({
		queryResult,
		columnProperties,
		itemAmount,
		pageAmount,
		items,
		sortDirection,
		toggleSorting,
		sortColumnKey,
		open,
		currentOpenIndex,
		highlightedItemId
	})}
		<div class="flex items-center justify-between">
			<div class="flex flex-1 items-center space-x-2">
				{@render headerFirst?.()}
				{#if config.enableSearch}
					<div class="relative max-w-sm">
						<Search class="absolute left-2 top-2.5 h-4 w-4 text-muted-foreground" />
						<Input
							bind:value={searchInput}
							placeholder="Search..."
							class="pl-8"
							oninput={() => {
								// Handle search input changes
								// This would need to be connected to the search parser
							}}
						/>
					</div>
				{/if}
				{@render headerAfterSearch?.()}
				{@render headerMiddle?.()}
			</div>
			<div class="flex items-center space-x-2">
				{#if queryResult.isLoading()}
					<div in:fade|local={{ duration: 100 }} out:fade|local={{ duration: 300 }}>
						<Loader2 class="h-4 w-4 animate-spin" />
					</div>
				{/if}
				{#if config.enablePagination && config.showTopPagination}
					{#if itemAmount >= 0}
						{@const startItemIndex = (currentPage - 1) * config.itemsPerPage + 1}
						{@const endItemIndex = clamp(
							currentPage * config.itemsPerPage,
							config.itemsPerPage,
							itemAmount
						)}

						<div class="flex items-center space-x-2">
							<p class="text-sm font-medium">
								{startItemIndex}-{endItemIndex} of {itemAmount}
							</p>
							<ShadcnDataTablePagination bind:currentPage {pageAmount} />
						</div>
					{:else}
						<div class="flex items-center space-x-2">
							<p class="text-sm font-medium">0-0 of {Math.max(0, itemAmount)}</p>
							<ShadcnDataTablePagination bind:currentPage {pageAmount} />
						</div>
					{/if}
				{/if}
			</div>
		</div>

		<Card>
			<Table class={classExport}>
				{#if config.showTableHeader}
					<TableHeader>
						<TableRow>
							<TableHead class="w-12">
								<Checkbox checked={selectAll} onclick={toggleSelectAll} />
							</TableHead>
							{#each Object.entries(columnProperties) as [key, colProp] (key)}
								{#if !colProp.hidden}
									<TableHead
										class={cn("whitespace-normal", key === 'actions' && "w-12")}
										onclick={() => colProp.sortable && toggleSorting(key)}
									>
										{#if colProp.sortable && items.length > 1}
											<button class="flex items-center space-x-2 hover:text-foreground">
												<span>{format(`dataTable.${config.type}.${key}.label`)}</span>
												{#if sortColumnKey === key && sortDirection === 'asc'}
													<ArrowUp class="h-4 w-4" />
												{:else if sortColumnKey === key && sortDirection === 'desc'}
													<ArrowDown class="h-4 w-4" />
												{:else}
													<ArrowUpDown class="h-4 w-4" />
												{/if}
											</button>
										{:else}
											<span>{format(`dataTable.${config.type}.${key}.label`)}</span>
										{/if}
									</TableHead>
								{/if}
							{/each}
							<TableHead class="w-12">
								<!-- Actions column -->
							</TableHead>
						</TableRow>
					</TableHeader>
				{/if}
				<TableBody>
					{#each items as item, index (item[config.dataUniquePropertyKey])}
						<ShadcnDataRow
							{item}
							{index}
							{config}
							openIndex={currentOpenIndex}
							{open}
							onClick={config.onItemClick}
							highlighted={highlightedItemId === item[config.dataUniquePropertyKey]}
							{striped}
							{hoverable}
							selected={isRowSelected(item)}
							onToggleSelection={toggleRowSelection}
						/>
					{/each}
				</TableBody>
			</Table>
		</Card>
		
		{#if items.length > 10 && config.enablePagination && config.showBottomPagination}
			<div class="flex items-center justify-between space-x-2 py-4" transition:fade|local={{ duration: 200 }}>
				<div class="flex-1 text-sm text-muted-foreground">
					{selectedRows.size} of {itemAmount} row(s) selected.
				</div>
				<div class="flex items-center space-x-2">
					<p class="text-sm font-medium">Rows per page</p>
					<Select value={config.itemsPerPage.toString()} class="h-8 w-[70px]">
						<option value="10">10</option>
						<option value="20">20</option>
						<option value="30">30</option>
						<option value="40">40</option>
						<option value="50">50</option>
					</Select>
					<div class="flex items-center space-x-2">
						<p class="text-sm font-medium">
							Page {currentPage} of {pageAmount}
						</p>
						<ShadcnDataTablePagination bind:currentPage {pageAmount} />
					</div>
				</div>
			</div>
		{/if}
	{/snippet}
</DataTable.Root>
