<script lang="ts">
	import Pagination from './ui/pagination.svelte';
	import PaginationContent from './ui/pagination-content.svelte';
	import PaginationItem from './ui/pagination-item.svelte';
	import PaginationLink from './ui/pagination-link.svelte';
	import ChevronLeft from './icons/chevron-left.svelte';
	import ChevronRight from './icons/chevron-right.svelte';
	import ChevronsLeft from './icons/chevrons-left.svelte';
	import ChevronsRight from './icons/chevrons-right.svelte';

	interface Props {
		currentPage: number;
		pageAmount: number;
	}

	let { currentPage = $bindable(), pageAmount }: Props = $props();

	let visiblePages = $derived(() => {
		const pages: number[] = [];
		const maxVisible = 5;
		const halfVisible = Math.floor(maxVisible / 2);

		let start = Math.max(1, currentPage - halfVisible);
		let end = Math.min(pageAmount, start + maxVisible - 1);

		if (end - start + 1 < maxVisible) {
			start = Math.max(1, end - maxVisible + 1);
		}

		for (let i = start; i <= end; i++) {
			pages.push(i);
		}

		return pages;
	});

	function goToPage(page: number) {
		if (page >= 1 && page <= pageAmount) {
			currentPage = page;
		}
	}

	function goToFirstPage() {
		currentPage = 1;
	}

	function goToLastPage() {
		currentPage = pageAmount;
	}

	function goToPreviousPage() {
		if (currentPage > 1) {
			currentPage--;
		}
	}

	function goToNextPage() {
		if (currentPage < pageAmount) {
			currentPage++;
		}
	}
</script>

<Pagination>
	<PaginationContent>
		<PaginationItem>
			<PaginationLink
				onclick={goToFirstPage}
				disabled={currentPage === 1}
				aria-label="Go to first page"
			>
				<ChevronsLeft class="h-4 w-4" />
			</PaginationLink>
		</PaginationItem>
		<PaginationItem>
			<PaginationLink
				onclick={goToPreviousPage}
				disabled={currentPage === 1}
				aria-label="Go to previous page"
			>
				<ChevronLeft class="h-4 w-4" />
			</PaginationLink>
		</PaginationItem>

		{#each visiblePages() as page}
			<PaginationItem>
				<PaginationLink
					onclick={() => goToPage(page)}
					isActive={currentPage === page}
					aria-label="Go to page {page}"
				>
					{page}
				</PaginationLink>
			</PaginationItem>
		{/each}

		<PaginationItem>
			<PaginationLink
				onclick={goToNextPage}
				disabled={currentPage === pageAmount}
				aria-label="Go to next page"
			>
				<ChevronRight class="h-4 w-4" />
			</PaginationLink>
		</PaginationItem>
		<PaginationItem>
			<PaginationLink
				onclick={goToLastPage}
				disabled={currentPage === pageAmount}
				aria-label="Go to last page"
			>
				<ChevronsRight class="h-4 w-4" />
			</PaginationLink>
		</PaginationItem>
	</PaginationContent>
</Pagination>
