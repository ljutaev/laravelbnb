<template>
	<div>
		<div v-if="!bookables">data is loading</div>
		<div>
			<div class="row mb-4 " v-for="row in rows" :key="'row'+row">
				<div class="col d-flex align-items-stretch" v-for="(bookable, column) in bookablesInRow(row)" :key="'row' + row + column">
					<bookable-list-item
						v-bind="bookable"
					>
					</bookable-list-item>
				</div>
				<div class="col d-flex align-items-stretch" v-for="p in placeholdersInRow(row)" :key="'placeholder'+ row + p"></div>
			</div>
			
		</div>			
	</div>
</template>

<script>
	import BookableListItem from "./BookableListItem"
	
	export default {
		components: {
			BookableListItem
		},
		data() {
			return {
				bookables: null,
				columns: 3
			}
		},
		computed: {
			rows() {
				return this.bookables == null ? 0 : Math.ceil(this.bookables.length / this.columns)
			}
		},
		methods: {
			bookablesInRow(row) {
				return this.bookables.slice( (row-1) * this.columns, row * this.columns)
			},
			placeholdersInRow(row) {
				return this.columns - this.bookablesInRow(row).length
			}
		},
		created() { 
			const request = axios
				.get('/api/bookables')
				.then(responce => (this.bookables = responce.data.data))
			console.log(request)
		}
	}
</script>