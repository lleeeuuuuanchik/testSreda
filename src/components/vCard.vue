<script setup>
    // components
    import vButton from './vButton.vue';

    // import usable functional
    import { defineProps, defineEmits, ref } from 'vue';

    // props
    const props = defineProps({
        obj: {
            type: Object,
            required: true,
        },
    });

    const copyObj = ref({...props.obj});

    // emits
    const emit = defineEmits(['delete', 'deleteNode', 'addNode', 'save']);

    // vars
    const isEditing = ref(false);

    // functions
    const deleteObject = () => emit('delete', props.obj.id);
    const deleteNode = (id) => emit('deleteNode', { id: props.obj.id, nodeId: id });
    const addNode = () => emit('addNode', props.obj.id);
    const saveElement = () => {
        isEditing.value = !isEditing.value;

        emit('save', { id: props.obj.id, title: copyObj.value.title, childs: copyObj.value.childs })
    }
</script>

<template>
    <div class="v-card">
        <div class="v-card__buttons-wr">
            <v-button v-if="isEditing" @click="addNode">
                Добавить узел
            </v-button>
            <v-button @click="deleteObject">
                Удалить
            </v-button>
            <v-button v-if="!isEditing" @click="isEditing = !isEditing">
                Редактировать
            </v-button>
            <v-button v-else @click="saveElement">
                Сохранить
            </v-button>
        </div>
        <div class="v-card__content">
            <h4 v-if="!isEditing" class="v-card__content-title">Наименование устройства: <span class="g-violet">{{ copyObj.title }}</span></h4>
            <input v-else v-model="copyObj.title" type="text" placeholder="Введите название" class="v-card__input">
            <div class="v-card__content-node-wr">
                <div
                    v-for="el in copyObj.childs"
                    :key="el.id"
                    class="v-card__content-node"
                >
                    <span v-if="!isEditing">Наименование узла: <span class="g-violet">{{ el.title }}</span></span>
                    <input v-else v-model="el.title" type="text" placeholder="Введите название" class="v-card__input">
                    <div class="v-card__buttons-wr">
                        <v-button v-if="isEditing" @click="deleteNode(el.id)">
                            Удалить
                        </v-button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang='scss'>
    .v-card {
        margin-bottom: 24px;
        padding: 20px;
        border: 2px solid #474747;
    }

    .v-card__buttons-wr {
        margin-bottom: 10px;
        display: flex;
        align-items: center;
        gap: 20px;
    }

    .v-card__content-title {
        margin-bottom: 10px;
        font-size: 18px;
    }

    .v-card__content-node-wr {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .v-card__content-node {
        padding: 20px;
        border: 2px solid #474747;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 20px;

        .v-card__buttons-wr { margin-bottom: 0; }
    }

    .v-card__input {
        margin-bottom: 10px;
        border: none;
        outline: none;
        background-color: transparent;
        color: white;
        padding: 10px 0;
        border-bottom: 2px solid #474747;
    }
</style>