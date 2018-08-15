<template>
	<thead class="thead" :class="{ 'table-dark': isDark }">
		<tr>
			<slot></slot>
		</tr>
	</thead>
</template>

<script>
export default {
	name: 'vui-dthead',
	props: {
		'dark': { type: Boolean, default: false },
		'labels': { type: Object, default: null },
		'sortby': { type: String, default: null }
	},
	data() {
		return {
			currentSort: this.sortby,
			currentSortDir: 'asc'
		}
	},
	computed: {
		isDark() {
			return !!this.dark
		}
	},
	methods:{
		sort(s, index) {	
			// reset sort icons for all but this col header
			let sortIconsLength = document.querySelectorAll('.sort-icons').length
			for (let i=0; i<sortIconsLength; i++) {
				if (i !== index) {
					document.querySelector(`#sortIcon${i}`).classList.remove('icon-caret-down')
					document.querySelector(`#sortIcon${i}`).classList.remove('icon-caret-up')
					document.querySelector(`#sortIcon${i}`).classList.add('icon-sort')
				}
			}

			// update the current sort direction
			if(s === this.currentSort) {
				this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
			}
			
			// set the curentSort to this s
			this.currentSort = s

			// change the sort icon for this col header
			if (this.currentSortDir === 'asc') {
				document.querySelector(`#sortIcon${index}`).classList.remove('icon-sort', 'icon-caret-down')
				document.querySelector(`#sortIcon${index}`).classList.add('icon-caret-up')
			}
			else if (this.currentSortDir === 'desc') {
				document.querySelector(`#sortIcon${index}`).classList.remove('icon-sort', 'icon-caret-up')
				document.querySelector(`#sortIcon${index}`).classList.add('icon-caret-down')
			}
		}
	}
}
</script>

<style scoped>
table th {
	cursor: pointer;
	border-top: 0px !important;
	padding-top: 30px;
}

.fa.caret-up, .fa.caret-down {
	font-size: 18px !important;
}
.fa.icon-sort {
	font-size: 14px !important;
}
</style>
