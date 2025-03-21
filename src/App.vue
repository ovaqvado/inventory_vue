<template>
	<div class="app_box">
		<div class="first_box">
			<left-panel />
			<inventory-box />
		</div>
		<div class="secondary_box"><footer-box /></div>
	</div>
</template>

<script setup lang="ts">
import FooterBox from './components/FooterBox.vue'
import InventoryBox from './components/InventoryBox.vue'
import LeftPanel from './components/LeftPanel.vue'
</script>

<style lang="scss" scoped>
.app_box {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	gap: 24px;
	height: 100%;
	width: 100%;
}

.first_box {
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: center;
	gap: 24px;
}
</style>
