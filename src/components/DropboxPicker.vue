<template>
  <div v-if="scriptLoaded && dropboxChooserIsSupported" @click="dropboxIconClicked">
    <slot/>
    <button v-if="!this.$slots.default">Open dropbox picker</button>
  </div>
</template>

<script>

export default {
  props  : {
    apiKey      : {
      type    : String,
      required: true
    },
    linkType    : {
      type   : String,
      default: 'preview'
    },
    multiselect : {
      type   : Boolean,
      default: false
    },
    extensions  : {
      type   : Array,
      default: function () {
        return [];
      }
    },
    folderselect: {
      type   : Boolean,
      default: false
    },
    sizeLimit   : {
      type   : Number,
      default: 0
    }
  },
  data   : () => ({
    scriptLoaded             : false,
    dropboxChooserIsSupported: false
  }),
  mounted() {
    if (window.Dropbox) {
      this.scriptLoaded = true
    } else {
      const dropBoxScript = document.createElement('script')
      dropBoxScript.onload = () => {
        this.scriptLoaded = true
        this.dropboxChooserIsSupported = window.Dropbox.isBrowserSupported()

        if (!this.dropboxChooserIsSupported) {
          console.warn('VueDropboxPicker: This browser is not supported')
        }
      }
      dropBoxScript.setAttribute('src', 'https://www.dropbox.com/static/api/2/dropins.js')
      dropBoxScript.setAttribute('id', 'dropboxjs')
      dropBoxScript.setAttribute('data-app-key', this.apiKey)
      document.head.appendChild(dropBoxScript)
    }
  },
  methods: {
    dropboxIconClicked() {
      let options = {
        success: async files => {
          let attachments = [];
          for (let i = 0; i < files.length; i++) {
            let attachment = {};
            attachment._id = files[i].id;
            attachment.title = files[i].name;
            attachment.size = files[i].bytes;
            attachment.iconURL = files[i].icon;
            attachment.link = files[i].link;
            attachment.extension = `. ${files[i].name.split(".")[1]}`;
            attachments.push(attachment);
          }
          this.tempAttachments = attachments;
          console.log(this.tempAttachments);
        },

        cancel: function () {
          console.log('cancel');
        },

        linkType   : this.linkType,
        multiselect: this.multiselect,
        folderselect: this.folderselect,
      }
      if (this.extensions.length) {
        options.extensions = this.extensions
      }

      if (this.sizeLimit) {
        options.sizeLimit = this.sizeLimit
      }
      window.Dropbox.choose(options);
    }
  }
}
</script>

<style></style>
