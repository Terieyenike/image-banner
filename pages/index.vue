<template>
  <div
    class="lg:flex lg:items-center px-8 py-8 max-w-md m-auto md:max-w-2xl lg:m-0 lg-p-0 w-full lg:max-w-5xl"
  >
    <div class="lg:w-2/4">
      <h1
        class="text-gray-900 text-lg mb-2 font-bold uppercase md:text-3xl lg:text-2xl"
      >
        Image Banner Generator
      </h1>
      <form @submit.prevent="handleBannerSubmit">
        <label class="mb-2 md:text-2xl lg:text-base"> Select an image </label>
        <div class="mb-4">
          <div class="flex items-center">
            <div
              v-for="banner in banners"
              :key="banner.id"
              @click.prevent="handleSelectedImage(banner.id, banner.publicId)"
              class="mr-2 shadow-md"
              :class="banner.id === imageId ? 'border-4 border-green-700' : ''"
            >
              <cld-image
                :public-id="banner.publicId"
                alt="laptop"
                responsive
                quality="auto"
              >
                <cld-transformation crop="fill" width="100" height="100"/>
              </cld-image>
            </div>
          </div>
          <p
            v-if="formData.error"
            class="text-red-700 mt-1 text-sm font-semibold md:text-lg lg:text-base"
          >
            Please select an image
          </p>
        </div>

        <div class="mb-8">
          <label class="block mb-3 md:text-2xl lg:text-base">
            Image Title
          </label>
          <input
            v-model="message"
            required
            name="message"
            class="border-gray-600 border rounded-sm p-2 w-full lg:w-3/4 md:text-lg"
          />
        </div>

        <!-- background colour -->
        <div class="flex items-center mb-4">
          <label class="mr-1 md:text-lg lg:text-base">Bg Color</label>
          <input
            type="color"
            :value="bgc.backgroundColour"
            @input.prevent="setBgColourChange($event)"
          />
        </div>

        <!-- overlay -->
        <div class="flex items-center mb-8">
          <label class="mr-1 md:text-lg lg:text-base"> Text color </label>
          <input
            type="color"
            :value="bgc.textColour"
            @input.prevent="setTextColourChange($event)"
          />
        </div>

        <button
          class="bg-green-700 text-white py-3 px-6 font-semibold tracking-normal md:text-2xl lg:text-base"
        >
          Generate Banner
        </button>
      </form>
    </div>

    <div v-if="showBanner" class="mt-10 lg:w-2/4 lg:mt-0">
      <Banner
        :message="message"
        :publicId="publicId"
        :bgcolour="bgc.backgroundColour"
        :textColour="bgc.textColour"
      />
    </div>
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
      banners,
      url: "",
      copy: "Copy",
      message: "",
      formData: {
        error: false,
      },
      bgc: {
        backgroundColour: "#242323",
        textColour: "#FFFFFF",
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
    setTextColourChange(event) {
      this.bgc.textColour = event.target.value;
      console.log(event.target.value);
    },
    handleBannerSubmit() {
      if (this.imageId) {
        this.showBanner = true;
      } else {
        this.formData.error = true;
      }
    },
  }
};
</script>
