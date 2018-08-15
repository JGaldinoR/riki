<template>
	<td>
		<slot></slot>
	</td>
</template>

<script>
export default {
	name: 'vui-dtbody-col',
	props: {

	},
	data() {
		return {
			
		}
	},
	mounted() {		
		//console.log(this.$parent.$parent.$el);
		
	},
	computed: {
		
	},
	methods: {
		// sort(s, index) {	
		// 	this.$parent.$parent.$emit('sort', s, index)
		// }
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