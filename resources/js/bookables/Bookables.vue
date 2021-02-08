<template>
	<div>
		<div v-if="!bookables">data is loading</div>
		<div>
			<bookable-list-item 
				:title="bookable.title" 
				:content="bookable.content" 
				:price="1000"
				v-for="(bookable, index) in bookables"
				:key="index"
			>
			</bookable-list-item>
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
			}
		},
		created() { 
			setTimeout(() => {
				this.bookables = [
					{
						title: "Item 1",
						content: "Content 1"
					},
					{
						title: "Item 2",
						content: "Content 2"
					}
				];
			}, 5000) 
		}
	}
</script>