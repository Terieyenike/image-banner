<template>
  <div class="mt-8">
    <cld-image :public-id="publicId" width="1000">
      <!-- <cld-transformation crop="fit" gravity="auto:subject" /> -->
      <!-- <cld-transformation effect="brightness_hsb:-50" /> -->
      <!-- <cld-transformation color="#FFFFFF" width="1300" crop="scale" /> -->
      <!-- <cld-transformation flags="layer_apply" /> -->
    </cld-image>
    <div class="mt-10">
      <h2 class="mb-2 text-gray-500 font-semibold">Copiable link</h2>
      <input
        :value="url"
        disabled
        class="w-full border-gray-300 rounded-sm border p-2"
      />
      <button
        @click="handleCopyLink"
        class="bg-green-400 py-3 px-6 font-semibold tracking-normal mt-6 text-gray-900"
      >
        {{ copy }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    message: String,
    publicId: String,
  },
  data() {
    return {
      url: "",
      copy: "Copy",
    };
  },
  methods: {
    handleCopyLink() {
      console.log("working!!!");
    },
  },
};
</script>
