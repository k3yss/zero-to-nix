<template>
  <div v-if="showBanner" class="fixed left-0 bottom-0 z-50">
    <div
      class="dark:bg-gray-accent-dark max-w-[202px] translate-x-[15px] translate-y-[-15px] rounded-lg bg-primary text-white/80"
    >
      <p class="m-0 p-3 text-[14px]">
        We use a single, first-party analytics cookie to focus our limited time
        and energy on the most important documentation. Check out our
        <a
          href="https://determinate.systems/privacy"
          class="underline"
          target="_blank"
          >privacy policy</a
        >.
      </p>
      <div class="grid grid-cols-2 divide-x border-t border-white/40">
        <button
          class="py-2 text-sm font-semibold text-white"
          @click="acceptCookies"
        >
          Accept
        </button>
        <button
          class="py-2 text-sm font-semibold text-white"
          @click="declineCookies"
        >
          Decline
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { posthog } from "posthog-js";
import { ref } from "vue";

const isEnabled = (import.meta.env.MODE === "production") &&
  (typeof window !== "undefined");

const showBanner = ref<boolean>(
  isEnabled &&
    !posthog.has_opted_out_capturing() &&
    !posthog.has_opted_in_capturing(),
);

const acceptCookies = () => {
  if (isEnabled) {
    posthog.opt_in_capturing();
  }

  showBanner.value = false;
};

const declineCookies = () => {
  if (isEnabled) {
    posthog.opt_out_capturing();
  }
  showBanner.value = false;
};
</script>