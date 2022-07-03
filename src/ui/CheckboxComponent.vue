<template>
	<div class="form-field form-checkbox">
		<label>
			<input
				type="checkbox"
				:value="inputValue"
				v-model="model"
			/>
			<span>{{ label }}</span>
		</label>

	</div>
</template>

<script>
export default {
	name: "CheckboxComponent",

	props: ['label', 'value', 'inputValue'],

	computed: {
		model: {
			get() {
				return this.value;
			},
			set(value) {
				this.$emit("input", value);
			},
		},
	},
};
</script>

<style lang="scss">
.form-checkbox input[type="checkbox"] {
	display: none;
}
.form-checkbox span {
	display: inline-block;
	vertical-align: middle;
	position: relative;
	padding-left: 34px;
	margin-right: 0;
	line-height: 18px;
	user-select: none;
	cursor: pointer;
}
.form-checkbox span:before {
	content: "";
	display: inline-block;
	vertical-align: middle;
	width: 18px;
	height: 18px;
	position: absolute;
	left: 0;
	top: -2px;
	background: #e6e6e6;
	border: 2px solid #b3b3b3;
	border-radius: 5px;
}

.form-checkbox input[type="checkbox"]:checked + span:before {
	background: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTAiIGhlaWdodD0iMTAiIHZpZXdCb3g9IjAgMCAxMCAxMCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNOS45MzEwOCAxLjE5MTAyTDMuNTU2MTYgNi4wOTEwN0wwLjA2NzIwNjEgMy4zMzc4TDAgNC42MTQ0OEwzLjQ2MzggOC44MDg5OEwxMCAyLjQ2NTk5TDkuOTMxMDggMS4xOTEwMloiIGZpbGw9IndoaXRlIi8+PC9zdmc+)
		center no-repeat #bfbfbf;
	background-size: 14px;
}
</style>