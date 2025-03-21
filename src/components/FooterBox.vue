<template>
	<div class="footer_box">
		<div class="ghost_footer"></div>
		<button class="btn_close_footer">
			<img src="../assets/image/close.svg" alt="close" />
		</button>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
	setup() {
		return {}
	},
})
</script>

<style lang="scss" scoped>
@use '../assets/global.scss' as *;

.footer_box {
	display: flex;
	align-items: center;
	position: relative;
	width: 785px;
	height: 72px;
	border: 1px solid $dark-border;
	border-radius: 12px;
}

.ghost_footer {
	margin-left: 18px;
	border-radius: 12px;
	width: 699px;
	height: 36px;
	background: $gradient-color;
}

.btn_close_footer {
	position: absolute;
	background: none;
	border: none;
	cursor: pointer;
	right: 14px;
	top: 14px;
}
</style>
