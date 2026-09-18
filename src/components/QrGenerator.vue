<template>
  <div class="p-8 max-w-lg mx-auto space-y-6">
    <h1 class="text-xl font-bold text-gray-800">Generate Custom QR Code with Logo</h1>

    <div class="space-y-4">
      <div>
        <label class="block text-sm font-medium mb-1">Text or Link:</label>
        <input
          v-model="options.data"
          @input="update"
          type="text"
          class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium mb-1">Pattern Color:</label>
          <input
            v-model="options.dotsColor"
            @input="update"
            type="color"
            class="w-full h-10 p-1 border rounded-lg cursor-pointer"
          />
        </div>
        <div>
          <label class="block text-sm font-medium mb-1">Background Color:</label>
          <input
            v-model="options.bgColor"
            @input="update"
            type="color"
            class="w-full h-10 p-1 border rounded-lg cursor-pointer"
          />
        </div>
      </div>

      <div>
        <label class="block text-sm font-medium mb-1">Dot Style:</label>
        <select
          v-model="options.dotsType"
          @change="update"
          class="w-full px-3 py-2 border rounded-lg bg-white"
        >
            <option value="square">Square</option>
            <option value="dots">Dots</option>
            <option value="rounded">Rounded</option>
            <option value="extra-rounded">Extra Rounded</option>
            <option value="classy">Classy</option>
        </select>
      </div>

      <div>
        <label class="block text-sm font-medium mb-1">Upload Center Logo:</label>
        <input
          type="file"
          accept="image/*"
          @change="onLogoSelected"
          class="w-full text-sm text-gray-500 file:py-2 file:px-4 file:rounded-lg file:border-0 file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100 cursor-pointer"
        />
        <button
          v-if="options.image"
          @click="removeLogo"
          type="button"
          class="text-xs text-red-600 hover:underline mt-1"
        >
          Remove Logo
        </button>
      </div>
    </div>

    <div v-if="options.image">
      <label class="block text-sm font-medium mb-1">
        Logo Size: {{ Math.round(options.imageSize * 100) }}%
      </label>
      <input 
        v-model.number="options.imageSize" 
        @input="update" 
        type="range" 
        min="0.1" 
        max="0.5" 
        step="0.05" 
        class="w-full accent-blue-600 cursor-pointer"
      />
    </div>

    <div class="flex flex-col items-center p-6 bg-gray-50 rounded-xl border space-y-4">
      <div ref="container" class="bg-white p-2 rounded-lg shadow-sm"></div>

      <div class="flex gap-3 w-full">
        <button
          @click="downloadQr('png')"
          class="flex-1 py-2 px-4 bg-blue-600 hover:bg-blue-700 text-white text-sm font-medium rounded-lg transition"
        >
          Download PNG
        </button>
        <button
          @click="downloadQr('svg')"
          class="flex-1 py-2 px-4 bg-gray-800 hover:bg-gray-900 text-white text-sm font-medium rounded-lg transition"
        >
          Download SVG
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import QRCodeStyling from "qr-code-styling";

const container = ref(null);

const options = reactive({
  data: "https://google.com",
  margin: 4,
  dotsColor: "#2563eb",
  dotsType: "rounded",
  bgColor: "#ffffff",
  cornersSquareType: "extra-rounded",
  cornerSquareColor: "#2563eb",
  cornerDotsType: "dot",
  cornerDotsColor: "#2563eb",
  image: "",
  imageSize: 0.35,
});

const qrCode = new QRCodeStyling({
  width: 240,
  height: 240,
  data: options.data,
  image: options.image,
  qrOptions: {
    errorCorrectionLevel: "H",
  },
  dotsOptions: {
    color: options.dotsColor,
    type: options.dotsType,
  },
  backgroundOptions: {
    color: options.bgColor,
  },
  imageOptions: {
    crossOrigin: "anonymous",
    margin: 4,
    imageSize: options.imageSize,
  },
  cornersSquareOptions: {
    type: "extra-rounded",
    color: options.dotsColor,
  },
});

onMounted(() => {
  qrCode.append(container.value);
});

const update = () => {
  qrCode.update({
    data: options.data,
    image: options.image,
    dotsOptions: {
      color: options.dotsColor,
      type: options.dotsType,
    },
    backgroundOptions: {
      color: options.bgColor,
    },
    imageOptions: {
      imageSize: options.imageSize,
    },
  });
};

const onLogoSelected = (event) => {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = () => {
    options.image = reader.result;
    update();
  };
  reader.readAsDataURL(file);
};

const removeLogo = () => {
  options.image = "";
  update();
};

const downloadQr = (ext) => {
  qrCode.download({
    name: "custom-qrcode",
    extension: ext,
  });
};
</script>