<template>
  <div class="py-8 max-w-sm m-auto px-8 md:px-0">
    <h1 class="text-gray-900 text-lg mb-2 font-bold uppercase">
      Image Banner Generator
    </h1>
    <form @submit.prevent="handleBannerSubmit">
      <label class="mb-2"> Select an image </label>
      <div class="mb-4">
        <div class="flex items-center">
          <div
            v-for="banner in banners"
            :key="banner.id"
            @click.prevent="handleSelectedImage(banner.id, banner.publicId)"
            class="mr-2 shadow-md"
            :class="banner.id === imageId ? 'border-4 border-green-700' : ''"
          >
            <cld-image :public-id="banner.publicId" alt="laptop">
              <cld-transformation crop="fill" width="80" height="80" />
            </cld-image>
          </div>
        </div>
        <p
          v-if="formData.error"
          class="text-red-700 mt-1 text-sm font-semibold"
        >
          Please select an image
        </p>
      </div>

      <div class="mb-8">
        <label class="block mb-3"> Image Title </label>
        <input
          v-model="message"
          required
          name="message"
          class="border-gray-600 border rounded-sm p-2 w-full"
        />
      </div>

      <!-- background colour -->
      <div class="flex items-center mb-4">
        <label class="mr-1">Bg Color</label>
        <input
          type="color"
          :value="bgc.backgroundColour"
          @input.prevent="setBgColourChange($event)"
        />
      </div>

      <!-- overlay -->
      <div class="flex items-center mb-8">
        <label class="mr-1"> Text overlay </label>
        <input type="color" :value="defaultValue" />
      </div>

      <button
        class="bg-green-700 text-white py-3 px-6 font-semibold tracking-normal"
      >
        Generate Banner
      </button>
    </form>

    <div v-if="showBanner" class="mt-10">
      <!-- <Banner :message="formData.text" :publicId="publicId" /> -->
      <div class="mt-8">
        <cld-image :public-id="publicId" width="1000">
          <cld-transformation crop="fill" gravity="auto:subject" />
          <cld-transformation effect="brightness_hsb:-20" />
          <cld-transformation
            :overlay="{
              fontFamily: 'Cookie',
              fontSize: 200,
              fontWeight: 'bold',
              text: message,
            }"
            color="#ffffff"
          />
          <cld-transformation flags="layer_apply" gravity="south" />
          <!-- <cld-transformation
            color="#FFFFFF"
            :overlay="{
              fontFamily: 'Inter',
              fontSize: 120,
              fontWeight: 'bold',
              text: 'message is not displaying',
              textAlign: 'center',
            }"
            width="1300"
            crop="fit"
          />
          <cld-transformation flags="layer_apply" /> -->
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
    </div>

    <!-- <div
      class="rounded-full w-80 h-80 bg-gray-300"
      :style="{ backgroundColor: this.bgc.backgroundColour }"
    ></div> -->
  </div>
</template>

<script>
import banners from "~/utils/banners.json";

export default {
  name: "IndexPage",
  data() {
    return {
      publicId: null,
      imageId: null,
      defaultValue: "#000000",
      banners,
      url: "",
      copy: "Copy",
      message: "",
      formData: {
        error: false,
      },
      bgc: {
        backgroundColour: "#000000",
      },
      showBanner: false,
    };
  },
  methods: {
    handleSelectedImage(imgId, publicId) {
      this.imageId = imgId;
      this.publicId = publicId;
      this.formData.error = false;
      this.showBanner = false;
    },
    setBgColourChange(event) {
      this.bgc.backgroundColour = event.target.value;
    },
    handleBannerSubmit() {
      if (this.imageId) {
        this.showBanner = true;
        this.message = "";
      } else {
        this.formData.error = true;
      }
    },
    handleCopyLink() {
      console.log("working!!!");
    },
  },
};
</script>
