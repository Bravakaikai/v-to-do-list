<template>
  <v-dialog
    v-model="dialog"
    width="350px"
  >
    <v-card>
      <v-card-title class="text-h6 grey lighten-2">
        {{ title }}
        <v-spacer/>
        <v-btn
          icon
          @click="dialog = false"
        >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text class="pa-4">
        {{ text }}
      </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-spacer/>
        <v-btn
          color="red"
          text
          @click="remove"
        >
          DELETE
        </v-btn>
        <v-btn
          color="grey"
          text
          @click="cancel"
        >
          CANCEL
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'Dialog',
  data () {
    return {
      dialog: false,
      title: null,
      text: null
    }
  },
  methods: {
    open (title, text) {
      this.title = title
      this.text = text
      this.dialog = true
      return new Promise((resolve) => {
        this.resolve = resolve
      })
    },
    remove () {
      this.resolve(true)
      this.dialog = false
    },
    cancel () {
      this.resolve(false)
      this.dialog = false
    }
  },
}
</script>
<style lang="scss" scoped>
.v-card__text {
  font-size: 16px;
}
</style>
