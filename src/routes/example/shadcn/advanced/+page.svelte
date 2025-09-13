<script lang="ts">
	import { DataTable } from '$lib/shadcn/index.js';
	import { LocalDataSource, ComponentType } from '$lib/index.js';
	import type { DataTableConfig } from '$lib/types/DataTableConfig.js';
	import usersData from '../../../../example-data/users.json';

	// Extract the items array from the JSON structure
	const users = usersData.items;

	const basicConfig: DataTableConfig<any> = {
		type: 'users',
		dataUniquePropertyKey: 'id',
		enableSearch: true,
		enablePagination: true,
		itemsPerPage: 5,
		showTableHeader: true,
		showTopPagination: true,
		showBottomPagination: true,
		columnProperties: {
			id: {
				propertyKey: 'id',
				componentType: ComponentType.NUMBER,
				sortable: true,
				hidden: false
			},
			userName: {
				propertyKey: 'userName',
				componentType: ComponentType.STRING,
				sortable: true,
				hidden: false
			},
			firstName: {
				propertyKey: 'firstName',
				componentType: ComponentType.STRING,
				sortable: true,
				hidden: false
			},
			lastName: {
				propertyKey: 'lastName',
				componentType: ComponentType.STRING,
				sortable: true,
				hidden: false
			},
			mailAddress: {
				propertyKey: 'mailAddress',
				componentType: ComponentType.STRING,
				sortable: true,
				hidden: false
			}
		},
		messageConfig: {
			id: {
				label: 'ID'
			},
			userName: {
				label: 'Username'
			},
			firstName: {
				label: 'First Name'
			},
			lastName: {
				label: 'Last Name'
			},
			mailAddress: {
				label: 'Email'
			}
		}
	};

	const stripedConfig: DataTableConfig<any> = {
		...basicConfig,
		type: 'users-striped'
	};

	const hoverableConfig: DataTableConfig<any> = {
		...basicConfig,
		type: 'users-hoverable'
	};

	const dataSource = new LocalDataSource(users, {
		filtering: {
			textSearchColumns: ['userName', 'firstName', 'lastName', 'mailAddress']
		}
	});
</script>

<div class="container mx-auto p-6 space-y-8">
	<h1 class="text-3xl font-bold">Shadcn DataTable Examples</h1>
	
	<div class="space-y-6">
		<div>
			<h2 class="text-xl font-semibold mb-3">Basic Table</h2>
			<p class="text-muted-foreground mb-4">
				A clean, modern data table with search and pagination.
			</p>
			<DataTable config={basicConfig} {dataSource} />
		</div>

		<div>
			<h2 class="text-xl font-semibold mb-3">Striped Table</h2>
			<p class="text-muted-foreground mb-4">
				Same table with alternating row colors for better readability.
			</p>
			<DataTable config={stripedConfig} {dataSource} striped={true} />
		</div>

		<div>
			<h2 class="text-xl font-semibold mb-3">Hoverable Table</h2>
			<p class="text-muted-foreground mb-4">
				Table with hover effects on rows.
			</p>
			<DataTable config={hoverableConfig} {dataSource} hoverable={true} />
		</div>
	</div>
</div>
