<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        bg-color="white"
        v-model="new_task"
        placeholder="Adicionar tafefa"
        dense
        @keyup.enter="add_task(new_task)"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="add_task(new_task)" />
        </template>
      </q-input>
    </div>

    <q-list separator bordered class="bg-white">
      <q-item
        v-for="(task, index) in todo_task"
        :key="task.title"
        v-ripple
        @click="task.done = !task.done"
        clickable
        :class="{ done: task.done }"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            dense
            color="negative"
            icon="delete"
            @click.stop="delete_task(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!todo_task.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No task</div>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue';
import { useQuasar } from 'quasar';

interface ITask {
  title: string;
  done: boolean;
}

const tasks = ref<ITask[]>([]);

export default defineComponent({
  name: 'IndexPage',

  setup() {
    const $q = useQuasar();
    let new_task = ref('');

    const delete_task = (index: number) => {
      $q.dialog({
        title: 'Apagar Tarefa',
        message: 'Você deseja realmente apagar essa tarefa ?',
        cancel: true,
        persistent: true,
      }).onOk(() => {
        tasks.value.splice(index, 1);
        $q.notify({
          message: 'A tarefa foi deletada',
          color: 'black',
          position: 'top',
        });
      });
    };

    const add_task = (task: string) => {
      tasks.value.push({ title: task, done: false });
      new_task.value = '';
    };

    return {
      todo_task: tasks,
      confirm: ref(false),
      delete_task,
      new_task,
      add_task,
    };
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
