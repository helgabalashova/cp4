<script>
import Edit from "./editImage.vue"
  export default {
    name: 'EditImage',
    props: ['currentImage'],
    components: {
        Edit,
    },
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>
<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal" role="dialog" aria-labelledby="modalTitle" aria-describedby="modalDescription">
        <header class="modal-header" id="modalTitle">
          <slot name="header">
                <h2>Edit Picture</h2>
                <button type="button" class="btn-close" @click="close" aria-label="Close modal">x</button>
          </slot>
        </header>
        <section class="modal-body" id="modalDescription">
          <slot name="body">
            <Edit v-bind:picture="currentImage"></Edit>
          </slot>
        </section>
        <footer class="modal-footer">
          <slot name="footer">
            <button class="close" type="button" @click="close" aria-label="Close modal">Close</button>
          </slot>
        </footer>
      </div>
    </div>
  </transition>
</template>
<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .close {
    padding: 8px 8px;
    background-color: #2a5d68;
    border: none;
    color: white;
    font-size: 15px;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    height: 55vh;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    display: grid;
    grid-template-columns: 6fr 1fr;
    align-items: center;
  }

  .modal-header h2 {
      font-size: 20px;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    background: transparent;
  }
</style>