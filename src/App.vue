<script setup>
	// components
	import vHeader from './components/vHeader.vue';
	import vCard from './components/vCard.vue';
	import vButton from './components/vButton.vue';

	// import usable functional
	import { ref } from 'vue';

	// тут не совсем понял о каких именно устройствах идет речь, но думаю, что это не сильно важно xD
	const data = ref([
		{
			id: crypto.randomUUID(),
			title: 'Устройство #1',
			childs: [
				{ id: crypto.randomUUID(), title: 'Узел1 для устройства #1', },
				{ id: crypto.randomUUID(), title: 'Узел2 для устройства #1', },
			],
		},
		{
			id: crypto.randomUUID(),
			title: 'Устройство #2',
			childs: [
				{ id: crypto.randomUUID(), title: 'Узел1 для устройства #2', },
				{ id: crypto.randomUUID(), title: 'Узел2 для устройства #2', },
			],
		},
	]);

	// functions
	const findElementIndex = (id) => {
		return data.value.findIndex(el => el.id === id);
	};

	const findElementNodeIndex = (idx, id) => {
		return data.value[idx].childs.findIndex(el => el.id === id);
	};

	const deleteElement = (id) => {
		const idx = findElementIndex(id);
		data.value.splice(idx, 1);
	};

	const deleteNodeElement = ({ id, nodeId }) => {
		const idx = findElementIndex(id);
		const nodeIdx = findElementNodeIndex(idx, nodeId);

		data.value[idx].childs.splice(nodeIdx, 1);
	};

	const addNewElement = () => {
		const obj = {
			id: crypto.randomUUID(),
			title: 'Новое устройство',
			childs: [],
		};

		data.value.push(obj);
	}

	const addNode = (id) => {
		const idx = findElementIndex(id);

		const obj = {
			id: crypto.randomUUID(),
			title: 'Новый узел',
		};

		data.value[idx].childs.push(obj);
	}

	const save = ({ id, title, childs }) => {
		// так то у меня в компоненте копия пропса изменяется, но на всякий просто меняю оригинальный обьект
		const idx = findElementIndex(id);

		data.value[idx].title = title;
		data.value[idx].childs = childs;
	}
</script>

<template>
	<div>
		<v-header />
		<main class="main container">
			<v-card
				v-for="el in data"
				:key="el.id"
				:obj="el"
				@delete="deleteElement"
				@deleteNode="deleteNodeElement"
				@addNode="addNode"
				@save="save"
			/>
			<v-button @click="addNewElement">Добавить новое устройство</v-button>
		</main>
	</div>
</template>

<style lang='scss'>
	.main { margin-top: 48px; }
</style>