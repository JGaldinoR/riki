<template>
	<div class="input-group mb-3">
		<input ref="input" type="text" class="form-control">
		<div class="input-group-append" v-if="showAddon">
			<span class="input-group-text">
				<i class="fa icon-calendar"></i>
			</span>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Datepicker',
	props: {
		'dateFormat': { type: String, default: 'mm/dd/yy' },
		'addon': { type: Boolean, default: false }
	},
  mounted: function() {		
		var self = this;
		$(this.$refs.input).datepicker({
			dateFormat: this.dateFormat,
			onSelect: function(date) {
				self.$emit('update-date', date);
			}
		});
	},
	computed: {
		showAddon() {
			return !!this.addon
		}
	},
  beforeDestroy: function() {
    $(this.$refs.input).datepicker('hide').datepicker('destroy');
  }
}
</script>

<style>

</style>

