<template>
  <BaseModal :isOpen="visible" :handleClose="() => close()">
    <template v-slot:title>{{ title }}</template>
    <template v-slot:content>
      <ProductItem :requisition="requisition" />
    </template>
  </BaseModal>
</template>

<script>
import { mapActions } from 'vuex';

import BaseModal from '@/components/template/modal/BaseModal.vue';
import ProductItem from '@/components/application/administrator/pretransfusion/productIntegrity/ProductItem.vue';
import Notification from '@/components/application/administrator/pretransfusion/Notification.vue';

export default {
  props: {
    requisition: {
      required: false,
      type: Object,
    },
    loading: {
      required: false,
    },
  },
  checkIntegrity(){},
  name: 'CheckIntegrityModal',
  components: {
    BaseModal,
    ProductItem,
    Notification,
  },
  data() {
    return {
      visible: true,
      dialog: false,
      unsubscribe: null,
      unsubscribeEdit: null,
      title: 'Conferência do Produto',
    };
  },
  methods: {
    ...mapActions('modal', [
      'closeCheckIntegrity',
      'rejectTransfusionRequisition',
    ]),
    close() {
      this.visible = false;
      this.closeCheckIntegrity();
    },
    handleReject() {
      this.cancelTransfusionRequisition();
    },
    openRejectModal() {
      this.rejectTransfusionRequisition();
    },
  },
};
</script>

<style>
</style>
