<template>
	<div>
		<bookable-list-item :title="bookable1.title" content="Content Props" :price="1000"></bookable-list-item>	
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
				bookable1:{
					title: "Item 1",
					content: "Content 1"
				},
				bookable2:{
					title: "Item 2",
					content: "Content 2"
				},
			}
		},
		created() {
			console.log('created')
			console.log(this.bookable1)
			console.log(this.bookable2)

			setTimeout(() => {
				this.bookable1.title = 'setTimeout'
			}, 5000)
		}
	}
</script>