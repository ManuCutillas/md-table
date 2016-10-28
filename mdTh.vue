<template>
	<th class="md-th" :class="align" ref="th">
		<span :class="getClass()" @click="$parent.$parent.orderBy(order);">
			<slot></slot>
			<md-icon v-if="order">{{ mode }}</md-icon>
		</span>
	</th>
</template>

<script>
	export default {
		props: {
			align: { type: String, required: false, default: 'left' },
			order: { type: String, required: false, default: null }
		},

		computed: {
			current() {
				return this.$parent.$parent.parameters.order == this.order;
			},

			mode() {
				return this.$parent.$parent.parameters.mode == 'asc'? 'arrow_upwards' : 'arrow_downward';
			}
		},

		methods: {
			getClass() {
				var classes = [];

				if (this.order)
					classes.push('sortable');

				if (this.current)
					classes.push('current');

				return classes.join(' ');
			}
		}
	};
</script>
