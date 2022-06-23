<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ formatted_string }}</div>
      </div>
      <q-img src="/cloud.jpg" class="header-image absolute-top"> </q-img>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="300"
      :breakpoint="600"
    >
      <q-scroll-area
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section> Todo </q-item-section>
          </q-item>

          <q-item to="/help" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section> Help </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="/cloud.jpg" style="height: 192px">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="https://avatars.githubusercontent.com/u/77557764?v=4" />
          </q-avatar>
          <div class="text-weight-bold">Lucas Valbusa</div>
          <div>@lucasvalbussa</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <Todo />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Todo from 'src/pages/Todo.vue';
import { date } from 'quasar';
import could_png from 'assets/could.png';

export default defineComponent({
  name: 'MainLayout',
  components: { Todo },

  setup() {
    const leftDrawerOpen = ref(false);
    const time_stamp = new Date();
    const formatted_string = date.formatDate(time_stamp, 'dddd D MMMM');

    return {
      leftDrawerOpen,
      formatted_string,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      could_png,
    };
  },
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.2;
  filter: grayscale(100%);
}
</style>
