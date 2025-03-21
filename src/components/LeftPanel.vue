<template>
	<div class="box_panel">
		<img
			class="image_panel"
			src="../assets/image/blur_photo_panel.png"
			alt="blur_panel"
		/>
		<div class="element_top"></div>
		<div class="elements">
			<div class="element_panel one"></div>
			<div class="element_panel two"></div>
			<div class="element_panel three"></div>
			<div class="element_panel four"></div>
			<div class="element_panel five"></div>
		</div>
		<div class="element_panel element_bottom six"></div>
	</div>
</template>

<script lang="ts">
export default {}
</script>

<style lang="scss" scoped>
@use '../assets/global.scss' as *;
.box_panel {
	display: flex;
	flex-direction: column;
	align-items: center;
	width: 236px;
	height: 500px;
	border: 1px solid #4d4d4d;
	border-radius: 12px;
	box-sizing: border-box;
}

.image_panel {
	width: 208px;
	height: 240px;
	margin-top: 18px;
	margin-bottom: 20px;
}

.element_top {
	border-radius: 8px;
	background: $gradient-color;
	height: 26px;
	width: 190px;
	margin-bottom: 24px;
}

.elements {
	display: flex;
	flex-direction: column;
	align-items: center;
	margin-bottom: 24px;
	gap: 16px;
}
.element_panel {
	background: $gradient-color;
	height: 10px;
	border-radius: 4px;
}
.one {
	width: 155px;
}

.two {
	width: 190px;
}

.three {
	width: 170px;
}

.four {
	width: 160px;
}

.five {
	width: 140px;
}

.element_bottom {
	width: 80px;
}
</style>
