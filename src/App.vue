<template>
  <div id="app">
    <VueDropboxPicker
        class="cloud dropbox"
        :api-key="'1231sfdf23424'"
        link-type="direct"
        :multiselect="true"
        :extensions="['.pdf', '.doc']"
        :folderselect="false"
        @cancel="onCancelDropbox"
        @picked="onPickedDropbox">
      <span>Open Dropbox Picker</span>
    </VueDropboxPicker>
  </div>
</template>

<script>
import VueDropboxPicker from './components/DropboxPicker.vue'

export default {
  name: 'app',
  methods: {
    onCancelDropbox (data) {
      window.console.log('on cancel:', data)
    },
    onPickedDropbox (data) {
      window.console.log('on picked:', data)
    }
  },
  components: {
    VueDropboxPicker
  }
}
</script>

