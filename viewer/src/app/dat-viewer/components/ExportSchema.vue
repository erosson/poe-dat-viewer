<template>
  <q-card style="width: 700px; max-width: 80vw;">
    <q-tabs v-model="format" :breakpoint="0"
      dense align="left" narrow-indicator
      class="text-grey" active-color="primary" indicator-color="primary"
    >
      <q-tab name="pypoe" label="PyPoE" />
      <q-tab name="pogo" label="pogo" />
      <q-tab name="c_struct" label="C struct" />
      <q-tab name="json" label="JSON" />
    </q-tabs>
    <q-separator />
    <q-tab-panels v-model="format" animated>
      <q-tab-panel name="pypoe" style="max-height: 60vh;">
        <div :class="$style.code">{{ pypoe }}</div>
      </q-tab-panel>
      <q-tab-panel name="pogo" style="max-height: 60vh;">
        <div :class="$style.code">{{ pogo }}</div>
      </q-tab-panel>
      <q-tab-panel name="c_struct" style="max-height: 60vh;">
        <div :class="$style.code">{{ clang }}</div>
      </q-tab-panel>
      <q-tab-panel name="json" style="max-height: 60vh;">
        <div :class="$style.code">{{ json }}</div>
      </q-tab-panel>
    </q-tab-panels>
    <q-separator />
    <q-card-actions align="right">
      <q-btn flat dense v-close-popup>Close</q-btn>
    </q-card-actions>
  </q-card>
</template>

<script>
import { exportToPogo } from './exporters/pogo'
import { exportToPypoe } from './exporters/pypoe'
import { exportToClang } from './exporters/c-struct'
import { exportInternalState } from './exporters/internal'

export default {
  inject: ['viewer'],
  data () {
    return {
      format: 'pypoe'
    }
  },
  computed: {
    pypoe () {
      const { headers, datFile } = this.viewer
      return exportToPypoe(headers, datFile.meta.name)
    },
    pogo () {
      const { headers, datFile } = this.viewer
      return exportToPogo(headers, datFile.meta.name)
    },
    clang () {
      const { headers, datFile } = this.viewer
      return exportToClang(headers, datFile.meta.name)
    },
    json () {
      const { headers, datFile } = this.viewer
      return exportInternalState(headers, datFile.meta.name)
    }
  }
}
</script>

<style lang="postcss" module>
.code {
  color: #fff;
  white-space: pre;
  padding: 16px;
  margin-bottom: 16px;
  border-radius: 4px;
  overflow-x: auto;
}
</style>
