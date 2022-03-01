<template>
  <div class="mt-8">
    <cld-image ref="ref" :public-id="publicId">

      <cld-transformation
        :background="bgColour"
        crop="fit"
        opacity="20"
        width="2000"
      />
      <cld-transformation
        :color="textColour"
        :overlay="{
          fontFamily: 'Arial',
          fontSize: 80,
          fontWeight: 'bold',
          text: message.toUpperCase(),
        }"
      />
            <cld-transformation flags="layer_apply"/>

    </cld-image>

    <div class="mt-10">
      <h2 class="mb-2 text-gray-500 font-semibold">Copiable link</h2>
      <input
        :value="url"
        class="w-full border-gray-300 rounded-sm border p-2"
        disabled
      />
      <button
        class="bg-green-400 py-3 px-6 font-semibold tracking-normal mt-6 text-gray-900"
        @click="handleCopyToClip"
      >
        {{ copy }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["message", "publicId", "bgColour", "textColour"],
  data() {
    return {
      url: "",
      copy: "Copy",
    };
  },
  methods: {
    handleCopyToClip() {
      navigator.clipboard
        .writeText(this.url)
        .then(() => (this.copy = "Copied!"))
        .catch((err) => console.log("error copying to clipboard", err));
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.url = this.$refs.ref.$el.src
    });
  },
};
</script>

