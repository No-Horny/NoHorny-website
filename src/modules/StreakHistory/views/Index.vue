<template>
  <div class="page">
    <h2>{{ t("YourStreakHistory") }}</h2>
    <streak-timer
      streaklabel="your current streak"
      :showActionButtons="false"
    />
    <div v-if="streaks.length > 0" class="have-streaks">
      <h2>{{ t("OtherStreaks") }}</h2>
      <div class="old-streaks">
        <old-streak-timer
          v-for="(streak, key) in streaks"
          :key="key.toString()"
          :startedTime="streak.startedAt"
          :endedTime="streak.endedAt"
          streaklabel="old streak"
        />
      </div>
    </div>
    <span class="have-not-streaks" v-else>{{
      t("YouHaveNotOldStreaksToSee")
    }}</span>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useI18n } from "vue-i18n";
import StreakTimer from "@/shared/components/streakTimer.vue";
import OldStreakTimer from "../components/oldStreakTimer.vue";

export default defineComponent({
  name: "StreakHistory",
  components: {
    StreakTimer,
    OldStreakTimer,
  },
  data() {
    const streaks = JSON.parse(localStorage.getItem("streakHistory") || "[]");
    const { t } = useI18n();

    return {
      t,
      streaks,
    };
  },
});
</script>

<style lang="scss" scoped>
.have-streaks {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 32px;
  h2 {
    margin-bottom: 12px;
  }
}
.have-not-streaks {
  margin-top: 8px;
  font-weight: 500;
}
.old-streaks {
  margin-top: 8px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16px;
}
</style>
