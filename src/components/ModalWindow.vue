<template>
	<transition name="fade">
		<div :class="{ modal: true, active: visible }" v-if="visible">
			<div class="modal-content">
				<button class="close_modal" @click="closeModal">
					<img src="../assets/image/close.svg" alt="" />
				</button>
				<div class="box_image">
					<img :src="item.image" alt="Item image" class="item_image" />
				</div>
				<div class="big_element"></div>
				<div class="elements_box">
					<div class="medium_element"></div>
					<div class="medium_element"></div>
					<div class="medium_element"></div>
					<div class="small_element"></div>
					<div class="extra_small_element"></div>
				</div>
				<div v-if="!confirming">
					<button class="delete_btn" @click="startRemoving">
						Удалить предмет
					</button>
				</div>
				<div class="window_confirm" v-else>
					<input
						placeholder="Введите количество"
						class="input_modal"
						type="number"
						v-model="inputCount"
						min="1"
					/>
					<div class="buttons_box">
						<button class="btn_cancel" @click="cancelRemoving">Отмена</button>
						<button class="btn_accept" @click="removeCount">Подтвердить</button>
					</div>
				</div>
			</div>
		</div>
	</transition>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
	props: {
		visible: { type: Boolean, required: true },
		onClose: { type: Function, required: true },
		item: { type: Object, required: true },
		updateCount: { type: Function, required: true },
	},
	setup(props) {
		const inputCount = ref()
		const confirming = ref(false)

		const startRemoving = () => {
			confirming.value = true
		}

		const removeCount = () => {
			if (inputCount.value > 0) {
				props.updateCount(props.item, inputCount.value)
				closeModal()
			}
		}

		const cancelRemoving = () => {
			confirming.value = false
		}

		const closeModal = () => {
			props.onClose()
			confirming.value = false
		}

		return {
			inputCount,
			confirming,
			startRemoving,
			removeCount,
			cancelRemoving,
			closeModal,
		}
	},
})
</script>

<style lang="scss" scoped>
@use '../assets/global.scss' as *;

.modal {
	box-sizing: border-box;
	position: absolute;
	top: 0;
	right: 0;
	width: 250px;
	height: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	background-color: #26262680;
	backdrop-filter: blur(16px);
	border-left: 1px solid $dark-border;
	padding: 0px;
	margin: 0px;
}

.modal-content {
	position: relative;
	display: flex;
	align-items: center;
	flex-direction: column;
	width: 100%;
	height: 100%;
	border-radius: 8px;
	padding: 0px;
	margin: 0px;
}
.box_image {
	box-sizing: border-box;
	display: flex;
	justify-content: center;
	align-items: center;
	height: 215px;
	width: 220px;
	border-bottom: 1px solid $dark-border;
	margin-bottom: 16px;
	.item_image {
		width: 130px;
		height: 130px;
	}
}

.big_element {
	width: 211px;
	height: 30px;
	border-radius: 8px;
	background: $gradient-color;
	margin-bottom: 24px;
}

.elements_box {
	width: 220px;
	display: flex;
	flex-direction: column;
	align-items: center;
	gap: 16px;
	padding-bottom: 24px;
	margin-bottom: 18px;
	border-bottom: 1px solid $dark-border;
	.medium_element {
		width: 211px;
		height: 10px;
		border-radius: 4px;
		background: $gradient-color;
	}
	.small_element {
		width: 180px;
		height: 10px;
		border-radius: 4px;
		background: $gradient-color;
	}
	.extra_small_element {
		width: 80px;
		height: 10px;
		border-radius: 4px;
		background: $gradient-color;
	}
}

.window_confirm {
	position: absolute;
	bottom: 0;
	width: 100%;
	height: 133px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	gap: 20px;
	background: $dark-bg-body;
	border-top: 1px solid $dark-border;
	box-sizing: border-box;
}

.input_modal {
	box-sizing: border-box;
	color: $dark-counter;
	appearance: none;
	-webkit-appearance: none;
	-moz-appearance: textfield;
	width: 210px;
	height: 40px;
	border: 1px solid $dark-border;
	border-radius: 8px;
	outline: none;
	background: none;
	padding-left: 12px;
	padding-right: 12px;
}

.delete_btn {
	background: #fa7272;
	border-radius: 8px;
	border: none;
	font-size: 14px;
	padding: 11px 55px 11px 57px;
	cursor: pointer;
	color: #ffffff;
	&:hover {
		background: #ff8888;
	}
}

.buttons_box {
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: center;
	gap: 10px;
	background: #26262699;
	backdrop-filter: blur(16px);
}
.btn_cancel {
	background: #ffffff;
	border: none;
	padding: 8px 19.5px 8px 19.5px;
	height: 33px;
	border-radius: 8px;
	box-sizing: border-box;
	cursor: pointer;
	&:hover {
		box-shadow: 0px 0px 10px 1px #fa727296;
	}
}

.btn_accept {
	color: #ffffff;
	background: #fa7272;
	border: none;
	padding: 8px 15px 8px 15px;
	height: 33px;
	border-radius: 8px;
	box-sizing: border-box;
	cursor: pointer;
	&:hover {
		box-shadow: 0px 0px 10px 1px #fa727296;
	}
}

.close_modal {
	position: absolute;
	top: 14px;
	right: 14px;
	background: none;
	border: none;
	cursor: pointer;
}
</style>
