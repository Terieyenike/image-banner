<template>
  <div class="mt-8">
      <cld-image ref="img" :public-id="publicId" responsive quality="auto">
        <cld-transformation
          :background="bgcolour"
          crop="fit"
          width="2000"
          opacity="20"
        />
        <!-- crop="fit"
          width="1500" -->
        <cld-transformation
          :overlay="{
          fontFamily: 'Arial',
          fontSize: 80,
          fontWeight: 'bold',
          text: message.toUpperCase(),
        }"
          :color="textColour"
        />
        <!-- crop="scale" -->
        <cld-transformation flags="layer_apply"/>
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
  props: ["message", "publicId", "bgcolour", "textColour"],
  data() {
    return {
      url: "",
      copy: "Copy",
    };
  },
  methods: {
    handleCopyLink() {
      navigator.clipboard
        .writeText(this.url)
        .then(() => (this.copy = "Copied!"))
        .catch((err) => console.log("error copying to clipboard", err));
    },
  },
  mounted() {
    this.$nextTick(() => {
      console.log(this.$refs.img.$el.className)
    });
  },
};
</script>
