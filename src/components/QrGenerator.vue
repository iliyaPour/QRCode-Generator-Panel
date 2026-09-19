<template>
  <div class="p-8 max-w-2xl mx-auto space-y-6">
    <h1 class="text-2xl font-bold text-gray-800">QR Code Studio</h1>

    <div class="space-y-4 bg-white p-6 rounded-xl border border-gray-200 shadow-sm">
      <div>
        <label class="block text-sm font-medium mb-1">Text or URL:</label>
        <input 
          v-model="options.data" 
          @input="update" 
          type="text" 
          class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:outline-none"
        />
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium mb-1">Dots Pattern:</label>
          <select v-model="options.dotsType" @change="update" class="w-full px-3 py-2 border rounded-lg bg-white">
            <option value="square">Square</option>
            <option value="dots">Dots</option>
            <option value="rounded">Rounded</option>
            <option value="extra-rounded">Extra Rounded</option>
            <option value="classy">Classy</option>
          </select>
        </div>
        <div>
          <label class="block text-sm font-medium mb-1">Dots Color:</label>
          <input v-model="options.dotsColor" @input="update" type="color" class="w-full h-10 p-1 border rounded-lg cursor-pointer" />
        </div>
      </div>

      <div>
        <label class="block text-sm font-medium mb-1">Margin: {{ options.margin }}px</label>
        <input v-model.number="options.margin" @input="update" type="range" min="0" max="30" step="2" class="w-full accent-blue-600 cursor-pointer" />
      </div>

      <hr class="my-4 border-gray-200" />

      <h3 class="font-semibold text-gray-700 text-sm">Corners Customization:</h3>
      
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-xs font-medium mb-1 text-gray-600">Corner Square Style:</label>
          <select v-model="options.cornerSquareType" @change="update" class="w-full px-3 py-2 border rounded-lg bg-white text-sm">
            <option value="square">Square</option>
            <option value="dot">Dot</option>
            <option value="extra-rounded">Extra Rounded</option>
          </select>
        </div>
        <div>
          <label class="block text-xs font-medium mb-1 text-gray-600">Corner Square Color:</label>
          <input v-model="options.cornerSquareColor" @input="update" type="color" class="w-full h-10 p-1 border rounded-lg cursor-pointer" />
        </div>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-xs font-medium mb-1 text-gray-600">Corner Dot Style:</label>
          <select v-model="options.cornerDotType" @change="update" class="w-full px-3 py-2 border rounded-lg bg-white text-sm">
            <option value="square">Square</option>
            <option value="dot">Dot</option>
          </select>
        </div>
        <div>
          <label class="block text-xs font-medium mb-1 text-gray-600">Corner Dot Color:</label>
          <input v-model="options.cornerDotColor" @input="update" type="color" class="w-full h-10 p-1 border rounded-lg cursor-pointer" />
        </div>
      </div>

      <hr class="my-4 border-gray-200" />

      <div>
        <label class="block text-sm font-medium mb-1">Center Logo:</label>
        <input 
          ref="fileInputRef"
          type="file" 
          accept="image/*" 
          @change="onLogoSelected" 
          class="w-full text-sm text-gray-500 file:py-2 file:px-4 file:rounded-lg file:border-0 file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100 cursor-pointer"
        />
        
        <div v-if="options.image" class="mt-3 space-y-2">
          <div class="flex justify-between items-center text-xs">
            <span>Logo Size: {{ Math.round(options.imageSize * 100) }}%</span>
            <button @click="removeLogo" type="button" class="text-red-600 hover:underline">Remove Logo</button>
          </div>
          <input 
            v-model.number="options.imageSize" 
            @input="update" 
            type="range" 
            min="0.1" 
            max="0.45" 
            step="0.05" 
            class="w-full accent-blue-600 cursor-pointer"
          />
        </div>
      </div>
    </div>

    <div class="flex flex-col items-center p-6 bg-gray-50 rounded-xl border border-gray-200 space-y-4">
      <div ref="container" class="bg-white p-2 rounded-lg shadow-sm"></div>

      <div class="flex gap-3 w-full max-w-xs">
        <button @click="downloadQr('png')" class="flex-1 py-2 bg-blue-600 hover:bg-blue-700 text-white text-sm font-medium rounded-lg transition">
          Download PNG
        </button>
        <button @click="downloadQr('svg')" class="flex-1 py-2 bg-gray-800 hover:bg-gray-900 text-white text-sm font-medium rounded-lg transition">
          Download SVG
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import QRCodeStyling from 'qr-code-styling'

const container = ref(null)
const fileInputRef = ref(null)

const options = reactive({
  data: 'https://google.com',
  margin: 4,
  dotsColor: '#1d4ed8',
  dotsType: 'rounded',
  cornerSquareType: 'extra-rounded',
  cornerSquareColor: '#1e3a8a',
  cornerDotType: 'dot',
  cornerDotColor: '#1e3a8a',
  image: '',
  imageSize: 0.35
})

const qrCode = new QRCodeStyling({
  width: 250,
  height: 250,
  data: options.data,
  margin: options.margin,
  image: options.image,
  qrOptions: {
    errorCorrectionLevel: 'H'
  },
  dotsOptions: {
    color: options.dotsColor,
    type: options.dotsType
  },
  backgroundOptions: {
    color: '#ffffff'
  },
  cornersSquareOptions: {
    type: options.cornerSquareType,
    color: options.cornerSquareColor
  },
  cornersDotOptions: {
    type: options.cornerDotType,
    color: options.cornerDotColor
  },
  imageOptions: {
    crossOrigin: 'anonymous',
    margin: 4,
    imageSize: options.imageSize
  }
})

onMounted(() => {
  if (container.value) {
    qrCode.append(container.value)
  }
})

const update = () => {
  qrCode.update({
    data: options.data,
    margin: options.margin,
    image: options.image,
    dotsOptions: {
      color: options.dotsColor,
      type: options.dotsType
    },
    backgroundOptions: {
      color: '#ffffff'
    },
    cornersSquareOptions: {
      type: options.cornerSquareType,
      color: options.cornerSquareColor
    },
    cornersDotOptions: {
      type: options.cornerDotType,
      color: options.cornerDotColor
    },
    imageOptions: {
      imageSize: options.imageSize
    }
  })
}

const onLogoSelected = (event) => {
  const file = event.target.files[0]
  if (!file) return

  const reader = new FileReader()
  reader.onload = () => {
    options.image = reader.result
    update()
  }
  reader.readAsDataURL(file)
}

const removeLogo = () => {
  options.image = ''
  if (fileInputRef.value) {
    fileInputRef.value.value = ''
  }
  update()
}

const downloadQr = (ext) => {
  qrCode.download({
    name: 'custom-qr',
    extension: ext
  })
}
</script>