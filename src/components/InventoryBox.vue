<template>
	<div class="box_inventory">
		<table>
			<tbody>
				<tr v-for="(row, rowIndex) in rows" :key="rowIndex">
					<td
						v-for="(cell, colIndex) in row"
						:key="colIndex"
						@dragover.prevent
						@drop="onDrop(rowIndex, colIndex)"
					>
						<div @click="openModal(cell)" class="element_box" v-if="cell">
							<img
								:src="cell.image"
								alt=""
								draggable="true"
								@dragstart="onDragStart(cell, rowIndex, colIndex)"
							/>
							<p class="counter">{{ cell.count }}</p>
						</div>
					</td>
				</tr>
			</tbody>
		</table>
		<modal-window
			class="modal"
			v-if="modalVisible"
			:visible="modalVisible"
			:item="selectedItem"
			:onClose="closeModal"
			:updateCount="updateCount"
		/>
	</div>
</template>

<script lang="ts">
import { onMounted, ref } from 'vue'
import greenElement from '../assets/image/green_element.png'
import orangeElement from '../assets/image/orange_element.png'
import purpleElement from '../assets/image/purple_element.png'
import ModalWindow from './ModalWindow.vue'

interface ElementItem {
	id: number
	image: string
	count: number
}

export default {
	components: { ModalWindow },

	setup() {
		const rows = ref<Array<Array<ElementItem | null>>>([
			[
				{ id: 1, image: greenElement, count: 4 },
				{ id: 2, image: orangeElement, count: 2 },
				{ id: 3, image: purpleElement, count: 5 },
				null,
				null,
			],
			[null, null, null, null, null],
			[null, null, null, null, null],
			[null, null, null, null, null],
			[null, null, null, null, null],
		])

		const modalVisible = ref(false)
		const selectedItem = ref<ElementItem | null>(null)

		const loadRowsFromLocalStorage = () => {
			const storedData = localStorage.getItem('inventoryRows')
			if (storedData) {
				rows.value = JSON.parse(storedData)
			}
		}

		const saveRowsToLocalStorage = () => {
			localStorage.setItem('inventoryRows', JSON.stringify(rows.value))
		}

		onMounted(() => {
			loadRowsFromLocalStorage()
		})

		const openModal = (item: ElementItem | null) => {
			selectedItem.value = item
			modalVisible.value = true
		}

		const closeModal = () => {
			modalVisible.value = false
			selectedItem.value = null
		}

		const updateCount = (item: ElementItem, countToRemove: number) => {
			const updatedItem = { ...item, count: item.count - countToRemove }

			rows.value.forEach((row, rowIdx) => {
				row.forEach((cell, colIdx) => {
					if (cell?.id === item.id) {
						if (updatedItem.count > 0) {
							rows.value[rowIdx][colIdx] = updatedItem
						} else {
							rows.value[rowIdx][colIdx] = null
						}
					}
				})
			})

			saveRowsToLocalStorage()
		}

		let draggedItem: ElementItem | null = null
		let draggedItemPosition: { rowIndex: number; colIndex: number } | null =
			null
		const onDragStart = (
			item: ElementItem,
			rowIndex: number,
			colIndex: number
		) => {
			draggedItem = item
			draggedItemPosition = { rowIndex, colIndex }
			setTimeout(() => {}, 0)
		}

		const onDrop = (rowIndex: number, colIndex: number) => {
			if (draggedItem) {
				const targetItem = rows.value[rowIndex][colIndex]
				rows.value[rowIndex][colIndex] = draggedItem
				if (draggedItemPosition) {
					if (targetItem) {
						rows.value[draggedItemPosition.rowIndex][
							draggedItemPosition.colIndex
						] = targetItem
					} else {
						rows.value[draggedItemPosition.rowIndex][
							draggedItemPosition.colIndex
						] = null
					}
				}
				draggedItem = null
				draggedItemPosition = null
				saveRowsToLocalStorage()
			}
		}
		return {
			onDragStart,
			onDrop,
			rows,
			modalVisible,
			openModal,
			closeModal,
			updateCount,
			selectedItem,
		}
	},
}
</script>

<style lang="scss" scoped>
@use '../assets/global.scss' as *;

.box_inventory {
	width: 525px;
	border: 1px solid $dark-border;
	border-radius: 12px;
	height: 500px;
	overflow: hidden;
	position: relative;
}

table {
	width: 100%;
	height: 100%;
	border-collapse: collapse;
}

td {
	border: 1px solid $dark-border;
	text-align: center;
	width: 105px;
	height: 100px;
	position: relative;
}

.element_box {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	position: relative;
	height: 100%;
	width: 100%;
	padding: 0;
	margin: 0;
	cursor: grab;
}

.counter {
	position: absolute;
	right: 0;
	bottom: 0;
	margin: 0;
	padding-left: 4px;
	padding-right: 4px;
	color: $dark-counter;
	border-top-left-radius: 4px;
	border-top: 1px solid $dark-border;
	border-left: 1px solid $dark-border;
	padding-top: 2px;
	padding-bottom: 2px;
}
.modal {
	animation: animation_modal 1s;
}

@keyframes animation_modal {
	0% {
		opacity: 0;
	}
	100% {
		opacity: 1;
	}
}
</style>
