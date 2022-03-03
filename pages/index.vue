<template>
  <div class="lg:flex lg:items-center px-8 py-8 max-w-md m-auto md:max-w-2xl lg:m-0 lg-p-0 w-full lg:max-w-5xl">

    <div class="lg:w-2/4">
      <h1 class="text-gray-900 text-lg mb-2 font-bold uppercase md:text-3xl lg:text-2xl">Image Banner Generator</h1>

      <form @submit.prevent="handleSubmit">
        <template v-if="banners.length > 0" class="mb-6">
          <label className="mb-2 md:text-2xl lg:text-base">
            Select an image
          </label>
          <div class="mb-4">
            <div class="flex items-center">
              <div
                v-for="item in banners"
                :key="item.id"
                :class="item.id === imageId ? 'border-4 border-green-700' : ''"
                class="mr-2 shadow-md"
                @click="handleSelectedImg(item.id, item.publicId)"
              >
                <cld-image :alt="item.alt" :public-id="item.publicId">
                  <cld-transformation crop="fill" height="100" width="100"/>
                </cld-image>
              </div>
            </div>

            <p v-if="formData.error" class="text-red-700 mt-1 text-sm font-semibold md:text-lg lg:text-base">
              Please select an image
            </p>
          </div>
        </template>

        <template v-else>Please include images in its library</template>
        <div class="mb-8">
          <label class="block mb-3 md:text-2xl lg:text-base">Image Title</label>
          <input
            v-model="formData.message"
            class="border-gray-600 border rounded-sm p-2 w-full lg:w-3/4 md:text-lg"
            name="message"
            required
          />

        </div>

        <div class="flex items-center mb-4">
          <label class="mr-1 md:text-lg lg:text-base">Bg Color</label>
          <input
            :value="formData.backgroundColour"
            type="color"
            @input.prevent="setBgColourChange($event)"
          />
        </div>

        <div class="flex items-center mb-8">
          <label class="mr-1 md:text-lg lg:text-base"> Text color </label>
          <input
            :value="formData.textColour"
            type="color"
            @input.prevent="setTextColourChange($event)"
          />
        </div>


        <button class="bg-green-700 text-white py-3 px-6 font-semibold tracking-normal md:text-2xl lg:text-base">
          Generate Banner
        </button>
      </form>
    </div>


    <div v-if="showBanner" class="mt-10 lg:w-2/4 lg:mt-0">
      <Banner
        :bgColour="formData.backgroundColour"
        :message="formData.message"
        :publicId="formData.publicId"
        :textColour="formData.textColour"
      />
    </div>
  </div>
</template>

<script>
import banners from "@/utils/banners.json";

export default {
  data() {
    return {
      banners,
      imageId: null,
      formData: {
        message: '',
        textColour: '#ffffff',
        backgroundColour: '#242323',
        publicId: null,
        error: false,
      },
      showBanner: false,
    }
  },

  methods: {
    handleSelectedImg(imgId, publicId) {
      this.imageId = imgId
      this.formData.publicId = publicId
      this.formData.error = false
      this.showBanner = false
    },

    handleSubmit() {
      if (this.imageId) {
        this.showBanner = true
      } else {
        this.formData.error = true
      }
    },
    setBgColourChange(event) {
      this.formData.backgroundColour = event.target.value;
    },
    setTextColourChange(event) {
      this.formData.textColour = event.target.value
    },
  },
}
</script>
