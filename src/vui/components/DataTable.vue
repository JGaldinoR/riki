<template>
	<vui-card 
		:id="`card-${id}`"
		class="shadow mb-3 p-0"
		:class="{ 'dark': isDark }" 
		@sort="sort"
	>
		<vui-card-body class="mt-0 p-0">
			<table
				:id="`table-${id}`" 
				class="table mt-0 pt-0" 
				:class="{ 
					'table-dark': isDark, 
					'table-striped': isStriped,
					'table-bordered': isBordered,
					'table-borderless': isBorderless,
					'table-hover': isHoverable,
					'table-sm': isSmall,
					'table-responsive': isResponsive,
					'scroll': isScrollable
				}"
			>
				<thead>
					<tr>
						<slot name="dthead-col"></slot>
						<th class="spacer" v-if="isScrollable"></th>
					</tr>
				</thead>
				<tfoot>
					<tr>
						<slot name="dtfoot-col"></slot>
						<th class="spacer" v-if="isScrollable"></th>
					</tr>
				</tfoot>
				<tbody :style="{'height': 49 * pageSize +'px'}">
					<tr v-for="(row, index) in sortedRows" :key="index">
						<slot name="dtbody-col" :row="row"></slot>
					</tr>
				</tbody>
			</table>

			<!-- PAGINATION -->
			<nav class="d-flex justify-content-center mt-3" v-if="isPagination">
				<ul class="pagination">
					<li class="page-item" :class="{ 'disabled': currentPage === 1 }">
						<a class="page-link" @click="prevPage">
							<span>&laquo;</span>
						</a>
					</li>
					
					<li 
						v-for="(page, index) in (totalRows / pageSize)" 
						:key="index"
						class="page-item"
						:class="{ 'active': currentPage === index + 1 }" 
						@click="thisPage(index + 1)"
					>
						<a class="page-link">{{ index + 1 }}</a>
					</li>
					
					<li 
						class="page-item" 
						:class="{ 'disabled': currentPage === (totalRows / pageSize) }"
					>
						<a class="page-link" @click="nextPage">
							<span>&raquo;</span>
						</a>
					</li>
				</ul>
			</nav>
		</vui-card-body>
	</vui-card>
</template>

<script>
export default {
	name: 'vui-data-table',
	props: {
		'id': { type: String },
		'dark': { type: Boolean, default: false },
		'striped': { type: Boolean, default: true },
		'border': { type: Boolean, default: false },
		'borderless': { type: Boolean, default: false },
		'hover': { type: Boolean, default: false },
		'sm': { type: Boolean, default: false },
		'responsive': { type: Boolean, default: false },
		'data': { type: Array },
		'sortby': { type: String },
		'cols': { type: Array },
		'scroll': { type: Boolean, default: false },
		'pagination': { type: Boolean, default: false },
		'pageSize': { type: String, default: '10' }
	},
	data() {
		let defaultSortBy = this.sortby
		let pgSize = this.pageSize
		const rows = this.data
		const rowsLength = this.data.length
		return {
			rows: rows,
			currentSort: defaultSortBy,
			currentSortDir: 'asc',
			pgSize: pgSize ,
  		currentPage: 1,
			totalRows: ''
		}
	},
	created() {
		this.totalRows = this.rows.length
	},
	mounted() {
		// add indexes to dthead columns
		let children = this.$children[0].$children[0].$children
		let childrenLength = children.length
		for (let n=0; n<childrenLength; n++) {
			children[n].$data.index = n
		}
	},
	computed: {
		isDark() {
			return !!this.dark
		},
		isStriped() {
			return !!this.striped
		},
		isBordered() {
			return !!this.border
		},
		isBorderless() {
			return !!this.borderless
		},
		isHoverable() {
			return !!this.hover
		},
		isSmall() {
			return !!this.sm
		},
		isResponsive() {
			return !!this.responsive
		},
		isScrollable() {
			return !!this.scroll
		},
		isPagination() {
			return !!this.pagination
		},
		sortedRows() {
			if (this.isPagination) {
				return this.rows.sort((a, b) => {
					let modifier = 1
					if (this.currentSortDir === 'desc') modifier = -1
					if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier
					if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier
					return 0
				}).filter((row, index) => {
					let start = (this.currentPage - 1) * this.pgSize
					let end = this.currentPage * this.pgSize
					if (index >= start && index < end) {
						return true
					}
				})
			}
			else {
				return this.rows.sort((a, b) => {
					let modifier = 1
					if (this.currentSortDir === 'desc') modifier = -1
					if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier
					if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier
					return 0
				})
			}
		}
	},
	methods: {
		sort(s, index, id) {
			s = s.toLowerCase()	
			// reset sort icons for all but this col header
			let sortIconsLength = document.querySelectorAll('.sort-icons').length
			for (let i=0; i<sortIconsLength; i++) {
				if (i !== index) {
					document.querySelector(`#sortIcon${i}${id}`).classList.remove('icon-caret-down')
					document.querySelector(`#sortIcon${i}${id}`).classList.remove('icon-caret-up')
					document.querySelector(`#sortIcon${i}${id}`).classList.add('icon-sort')
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
				document.querySelector(`#sortIcon${index}${id}`).classList.remove('icon-sort', 'icon-caret-down')
				document.querySelector(`#sortIcon${index}${id}`).classList.add('icon-caret-up')
			}
			else if (this.currentSortDir === 'desc') {
				document.querySelector(`#sortIcon${index}${id}`).classList.remove('icon-sort', 'icon-caret-up')
				document.querySelector(`#sortIcon${index}${id}`).classList.add('icon-caret-down')
			}
		},
		prevPage() {
  		if (this.currentPage > 1) {
				this.currentPage--
			}
		},
		thisPage(page) {
			this.currentPage = page
		},
		nextPage() {
			if ((this.currentPage * this.pgSize) < this.rows.length) {
				this.currentPage++
			}
		} 
	}
}
</script>

<style scoped>
.dark {
	color: #fff;
  background-color: #212529;
}

.disabled, .disabled:hover {
	cursor: default;
}

/* TABLE STYLES *****************************/
table {
	margin-top: 0;
	margin-bottom: 0;
}

table th {
	cursor: pointer;
	border-top: 0px !important;
	padding-top: 30px;
}

/* SCROLL TABLE STYLES *********************/
table.scroll {
	display: table;
}

/* THEAD */
table.scroll thead tr {
	display: table;
	width: 100%;
	table-layout: fixed;
}

table.scroll thead tr th:nth-last-child(2) {
	border-right: 0px !important;
}

table.scroll thead tr th.spacer {
	width: 15px;
	border-left: 0px !important;
	padding: 0 !important;
}

/* TFOOT */
table.scroll tfoot tr {
	display: table;
	width: 100%;
	table-layout: fixed;
}

table.scroll tfoot tr th:nth-last-child(2) {
	border-right: 0px !important;
}

table.scroll tfoot tr th.spacer {
	width: 15px;
	border-left: 0px !important;
	padding: 0 !important;
}


/* TBODY */
table.scroll tbody {
	overflow-y: auto;
	overflow-x: hidden;
	display: block;
	width: 100%;
}

table.scroll tbody tr {
	display: table;
	width: 100%;
	table-layout: fixed;
}



.fa.caret-up, .fa.caret-down {
	font-size: 18px !important;
}
.fa.icon-sort {
	font-size: 14px !important;
}

</style>
