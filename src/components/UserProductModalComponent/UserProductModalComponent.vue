<script>
import Modal from 'bootstrap/js/dist/modal';

const { VITE_URL, VITE_PATH } = import.meta.env;

export default {
  props: {
    tempProductId: {
      type: String,
      required: true,
    },
    addToCart: {
      type: Function,
    },
  },
  data() {
    return {
      modal: {},
      product: {},
      qty: 1,
    };
  },
  watch: {
    tempProductId() {
      this.$http
        .get(`${VITE_URL}/api/${VITE_PATH}/product/${this.tempProductId}`)
        .then((res) => {
          this.product = res.data.product;
          this.openModal();
        })
        .catch((err) => {
          alert(err.response.data.message);
        });
    },
  },
  methods: {
    openModal() {
      this.modal.show();
    },
    closeModal() {
      this.modal.hide();
    },
  },
  mounted() {
    this.modal = new Modal(this.$refs.userProductModal);
  },
};
</script>

<template src="./UserProductModalComponent.html"></template>
