<script lang="ts">
	import { DataTable } from '$lib/shadcn/index.js';
	import { LocalDataSource, ComponentType } from '$lib/index.js';
	import type { DataTableConfig } from '$lib/types/DataTableConfig.js';
	import usersData from '../../../example-data/users.json';

	// Extract the items array from the JSON structure
	const users = usersData.items;

	const config: DataTableConfig<any> = {
		type: 'users',
		dataUniquePropertyKey: 'id',
		enableSearch: true,
		enablePagination: true,
		itemsPerPage: 10,
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

	const dataSource = new LocalDataSource(users, {
		filtering: {
			textSearchColumns: ['userName', 'firstName', 'lastName', 'mailAddress']
		}
	});
</script>

<div class="container mx-auto p-6">
	<h1 class="text-3xl font-bold mb-6">Shadcn DataTable Example</h1>
	
	<div class="mb-6">
		<h2 class="text-xl font-semibold mb-3">Basic Usage</h2>
		<p class="text-muted-foreground mb-4">
			This example demonstrates the Shadcn-styled DataTable component with modern UI components.
		</p>
	</div>

	<DataTable {config} {dataSource} />
</div>
