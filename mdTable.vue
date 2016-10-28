<template>
	<md-whiteframe md-elevation="1">
		<md-toolbar class="md-table-title">
			<slot name="title"></slot>

			<div v-show="selecteds">
				<slot name="actions"></slot>
			</div>
		</md-toolbar>

		<table class="md-table" ref="table">
			<slot name="headers"></slot>

			<slot></slot>

			<tfoot>
				<tr>
					<td :colspan="numColumns">footer</td>
				</tr>
			</tfoot>
		</table>
		{{ idField }}
	</md-whiteframe>
</template>

<script>
	export default {
		props: {
			title: { type: String, required: false, default: '' },
			update: { type: Function, required: false },
			parameters: { type: Object, required: false },
			selecteds: { type: Array, required: false }
		},

		data() {
			return {
				numColumns: 0,
				idField: 'id',
				idDefined: false
			};
		},

		mounted() {
			this.numColumns = this.$refs.table.children[0].children[0].childElementCount;
		},

		updated() {
			if (!this.selecteds || this.idDefined)
				return;

			for (var slot in this.$slots.default) {
				var tbody = this.$slots.default[slot];

				if (tbody.tag == 'tbody' && tbody.children.length) {
					console.log(this);

					if (tbody.children[0].key)
						this.idField = tbody.children[0].key;

					this.idDefined = true;

					break;
				}
			}
		},

		methods: {
			orderBy(order) {
				if (this.parameters.order != order)
					this.parameters.order = order;

				else if (this.parameters.mode == 'asc')
					this.parameters.mode = 'desc';

				else
					this.parameters.mode = 'asc';

				this.update();
			}
		}
	};
</script>

<style lang="scss">
	.md-toolbar.md-table-title {
		padding: 0 24px;

		h2.md-title {
			margin: 0;
		}
	}

	table.md-table {
		width: 100%;
		color: rgba(0, 0, 0, .87);
		border-collapse: collapse;
		border-spacing: 0;

		tr {
			th.md-th,
			td.md-td {
				text-align: left;
				padding: 0 24px;

				&.right {
					text-align: right;
				}
			}
		}


		thead {
			font-size: 12px;
			font-weight: 700;
			color: rgba(0, 0, 0, .54);

			th.md-th {
				height: 56px;

				.sortable {
					cursor: pointer;

					i.md-icon {
						margin-top: -3px;
						min-width: 0;
						width: 16px;
						min-height: 0;
						font-size: 16px;
						color: rgba(0, 0, 0, 0);

					}

					&:not(.current):hover i.md-icon {
						color: rgba(0, 0, 0, .38);
					}

					&.current {
						color: rgba(0, 0, 0, .87);

						& i.md-icon {
							color: rgba(0, 0, 0, .87);
						}
					}
				}
			}
		}

		tbody {
			font-size: 13px;

			tr {
				td {
					height: 48px;
				}

				&:hover td {
					background-color: #eee;
				}

				&.selected td {
					background-color: #f5f5f5;
				}
			}
		}

	}
</style>
