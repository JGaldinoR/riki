<template>
	<th 
		@click="isSortable ? sort(sortby, index) : null"
		class="p-2"
		:class="{ 'pointer': isSortable }"
		:style="{ 'width': width }"
		scope="col"
	>
		<slot></slot>
		<i 
			:id="`sortIcon${index}${$parent.$parent.$attrs.id}`" 
			class="sort-icons ml-1 fa icon-sort"
			v-show="isSortable"
		></i>
	</th>
</template>

<script>
export default {
	name: 'vui-dthead-col',
	props: {
		'sortby': { type: String },
		'width': { type: String, default: 'auto' }
	},
	data() {
		return {
			index: null
		}
	},
	mounted() {		
		//console.log(this.$parent.$parent.$el);
		//console.log(this.$parent.$parent.$attrs.id);
	},
	computed: {
		isSortable() {
			return this.sortby ? true : false
		}
	},
	methods: {
		sort(s, index) {	
			this.$parent.$parent.$emit('sort', s, index, this.$parent.$parent.$attrs.id)
		}
	}
}
</script>

<style scoped>
table th {
	border-top: 0px !important;
	padding-top: 30px;
}

.pointer {
	cursor: pointer;
}

.fa.caret-up, .fa.caret-down {
	font-size: 18px !important;
}
.fa.icon-sort {
	font-size: 14px !important;
}
</style>