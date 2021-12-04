<template>
  <div>
    <v-btn
      v-if="options.edit"
      icon
      @click="btnHandler('edit')"
    >
      <v-icon>mdi-pencil</v-icon>
    </v-btn>
    <v-btn
      v-if="options.submit"
      icon
      @click="btnHandler('submit')"
    >
      <v-icon>mdi-check</v-icon>
    </v-btn>
    <v-btn 
      v-if="options.cancel"
      icon
      @click="btnHandler('cancel')"
    >
      <v-icon>mdi-close</v-icon>
    </v-btn>
    <v-btn
      v-if="options.remove"
      icon
      @click="btnHandler('remove')"
    >
      <v-icon>mdi-trash-can</v-icon>
    </v-btn>
  </div>
</template>
<script>
export default {
  name: '_EditBtn',
  props: {
    options: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  methods: {
    btnHandler (type) {
      this.$emit(type)
    }
  }
}
</script>
