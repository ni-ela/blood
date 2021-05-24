<template>
  <v-card flat  width="518" class="ml-4">
    <v-list subheader>
      <v-subheader class="text-subtitle-1 pl-0"
        >Aspectos de não conformidade nesse produto:</v-subheader
      >
      <v-list-item-group v-model="settings" multiple>
        <v-list-item class="pl-0">
          <template v-slot:default="{ active }">
            <v-list-item-action class="mr-1">
              <v-checkbox :input-value="active" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title>Hemólise</v-list-item-title>
            </v-list-item-content>
          </template>
        </v-list-item>
        
      </v-list-item-group>
        <v-btn
          class="text-center text-uppercase mt-3"
          color="error"
          large
          block
          depressed
          dark
            @click="console.log('hi')"
        >
          REPORTAR INTEGRIDADE
        </v-btn>
    </v-list>
  </v-card>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  props: {
    requisitionAttendanceSummary: {
      type: Object,
      required: false,
    },
  },
  data() {
    return {};
  },
  methods: {
    ...mapActions('modal', ['checkIntegrity']),
    parseDate(value) {
      return parseDateToDDMMYYYY(value);
    },
  },
};
</script>

<style>
</style>