<template>
  <modal
    :class="showChooseBreathingModal ? 'open' : 'closed'"
    @close="$emit('close')"
  >
    <header class="modal-header">
      <h3>{{ t("ChooseABreathingTechnique") }}</h3>
    </header>
    <div class="modal-body" v-if="showChooseBreathingModal">
      <div
        class="breathing-picker"
        v-for="(technique, index) in techniques"
        :key="index"
      >
        <button @click="technique.showPreBreathingModal = true">
          {{ technique.name[userPreferences.lang] }}
        </button>

        <pre-breathing-modal
          :breathingTechniqueIndex="index"
          :showBreathingModal="technique.showPreBreathingModal"
          @close="technique.showPreBreathingModal = false"
          @start="technique.showBreathingModal = true"
        />
        <breathing-modal
          :breathingTechniqueIndex="index"
          :showBreathingModal="technique.showBreathingModal"
          @close="technique.showBreathingModal = false"
        />
      </div>
      <footer>
        <button @click="$emit('close')" class="close">{{ t("Close") }}</button>
      </footer>
    </div>
  </modal>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useI18n } from "vue-i18n";
import Modal from "@/shared/components/appModal.vue";
import BreathingModal from "./breathingModal.vue";
import PreBreathingModal from "./preBreathingModal.vue";
import { userPreferences } from "@/shared/user-preferences";
import { techniques } from "./techniques";

export default defineComponent({
  name: "ChooseABreathingModal",
  components: {
    Modal,
    BreathingModal,
    PreBreathingModal,
  },
  data() {
    const { t } = useI18n();

    return {
      t,
      techniques,
      userPreferences,
      showBreathingTechniqueModal1: false,
      showBreathingTechniqueModal2: false,
    };
  },
  props: {
    showChooseBreathingModal: {
      type: Boolean,
      required: true,
    },
  },
});
</script>

<style lang="scss" scoped>
.modal-body {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  .breathing-picker {
    width: 100%;
    button {
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 6px 8px;
      font-weight: 500;
      background: #ddd;
      color: #24242d;
      border: 0;
      border-radius: 6px;
      transition: filter 0.2s;
      :hover {
        filter: brightness(0.8);
      }
    }
  }
}
</style>
