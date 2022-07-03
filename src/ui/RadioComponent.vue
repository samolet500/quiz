<template>
	<div class="form-field form-radio">
		<label>
			<input
			type="radio"
			:id="value"
			:name="name"
			:value="value"
			:checked="isChecked"
			@change="$emit('change', $event.target.value)"
		/>
			<span>{{ label }}</span>
		</label>
	</div>
</template>

<script>
export default {
	name: "RadioComponent",

	props: ["name", "value", "label", "modelValue"],

	computed: {
		isChecked() {
			return this.modelValue === this.value;
		},
	},

	emits: ["change"],

	model: {
		prop: "modelValue",
		event: "change",
	},
};
</script>

<style lang="scss">
.form-radio input[type="radio"] {
	display: none;
}
.form-radio span {
	display: inline-block;
	vertical-align: middle;
	position: relative;
	padding-left: 34px;
	margin-right: 0;
	line-height: 18px;
	user-select: none;
	cursor: pointer;
}
.form-radio span:before {
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
	border-radius: 50%;
}

.form-radio input[type="radio"]:checked + span:before {
	background: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTAiIGhlaWdodD0iMTAiIHZpZXdCb3g9IjAgMCAxMCAxMCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNOS45MzEwOCAxLjE5MTAyTDMuNTU2MTYgNi4wOTEwN0wwLjA2NzIwNjEgMy4zMzc4TDAgNC42MTQ0OEwzLjQ2MzggOC44MDg5OEwxMCAyLjQ2NTk5TDkuOTMxMDggMS4xOTEwMloiIGZpbGw9IndoaXRlIi8+PC9zdmc+)
		center no-repeat #bfbfbf;
	background-size: 14px;
}
</style>