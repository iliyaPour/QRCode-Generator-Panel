<template>
  <div class="min-h-screen bg-slate-50 py-10 px-4 sm:px-6 lg:px-8">
    <div class="max-w-5xl mx-auto space-y-6">
      <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 pb-6 border-b border-slate-200">
        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold text-slate-900 tracking-tight">QR Code Studio</h1>
          <p class="text-sm text-slate-500 mt-1">Design, customize, and export high-resolution QR codes</p>
        </div>
        <button
          @click="resetToDefaults"
          type="button"
          class="inline-flex items-center justify-center gap-2 px-4 py-2 text-sm font-medium text-slate-600 bg-white border border-slate-300 rounded-xl hover:bg-rose-50 hover:text-rose-600 hover:border-rose-200 transition-colors shadow-sm cursor-pointer"
        >
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
          </svg>
          Reset to Defaults
        </button>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
        <div class="lg:col-span-7 space-y-5">
          <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-2">
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600">Content</label>
            <input
              v-model="options.data"
              @input="update"
              type="text"
              placeholder="https://example.com"
              class="w-full px-4 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all text-slate-800"
            />
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800">Body & Pattern</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Dot Style</label>
                <select
                  v-model="options.dotsType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dots">Dots</option>
                  <option value="rounded">Rounded</option>
                  <option value="extra-rounded">Extra Rounded</option>
                  <option value="classy">Classy</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Pattern Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-xl">
                  <input
                    v-model="options.dotsColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 uppercase">{{ options.dotsColor }}</span>
                </div>
              </div>
            </div>

            <div class="bg-slate-50/80 p-3.5 rounded-xl border border-slate-200/80 space-y-2.5">
              <div class="flex justify-between items-center text-xs">
                <span class="font-medium text-slate-700">Margin Padding</span>
                <span class="px-2.5 py-0.5 font-mono text-xs font-bold text-indigo-700 bg-indigo-50 border border-indigo-100 rounded-full">
                  {{ options.margin }}px
                </span>
              </div>
              <div class="flex items-center gap-3">
                <span class="text-[10px] font-semibold text-slate-400">0</span>
                <input
                  v-model.number="options.margin"
                  @input="update"
                  type="range"
                  min="0"
                  max="30"
                  step="2"
                  class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer 
                         [&::-webkit-slider-runnable-track]:rounded-lg [&::-webkit-slider-runnable-track]:bg-slate-200
                         [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:w-5 [&::-webkit-slider-thumb]:h-5 
                         [&::-webkit-slider-thumb]:bg-white [&::-webkit-slider-thumb]:border-2 [&::-webkit-slider-thumb]:border-indigo-600 
                         [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:shadow-md 
                         [&::-webkit-slider-thumb]:transition-transform [&::-webkit-slider-thumb]:duration-150 
                         hover:[&::-webkit-slider-thumb]:scale-110 active:[&::-webkit-slider-thumb]:scale-95 focus:outline-none"
                />
                <span class="text-[10px] font-semibold text-slate-400">30</span>
              </div>
            </div>
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800">Corners Styling</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Outer Corner Box</label>
                <select
                  v-model="options.cornerSquareType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dot">Dot</option>
                  <option value="extra-rounded">Extra Rounded</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Outer Corner Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-xl">
                  <input
                    v-model="options.cornerSquareColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 uppercase">{{ options.cornerSquareColor }}</span>
                </div>
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Inner Corner Dot</label>
                <select
                  v-model="options.cornerDotType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dot">Dot</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Inner Corner Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-xl">
                  <input
                    v-model="options.cornerDotColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 uppercase">{{ options.cornerDotColor }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800">Center Logo</h3>
            </div>

            <div>
              <input
                ref="fileInput"
                type="file"
                accept="image/*"
                @change="onLogoSelected"
                class="w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-indigo-50 file:text-indigo-700 hover:file:bg-indigo-100 cursor-pointer transition-all"
              />
            </div>

            <div v-if="options.image" class="pt-3 border-t border-slate-100 space-y-3">
              <div class="flex justify-between items-center text-xs">
                <span class="font-medium text-slate-700">Logo Scale</span>
                <div class="flex items-center gap-2">
                  <span class="px-2.5 py-0.5 font-mono text-xs font-bold text-indigo-700 bg-indigo-50 border border-indigo-100 rounded-full">
                    {{ Math.round(options.imageSize * 100) }}%
                  </span>
                  <button
                    @click="removeLogo"
                    type="button"
                    class="text-xs font-semibold text-rose-500 hover:text-rose-700 hover:bg-rose-50 px-2 py-0.5 rounded-md transition cursor-pointer"
                  >
                    Remove
                  </button>
                </div>
              </div>

              <div class="flex items-center gap-3">
                <span class="text-[10px] font-semibold text-slate-400">10%</span>
                <input
                  v-model.number="options.imageSize"
                  @input="update"
                  type="range"
                  min="0.1"
                  max="0.45"
                  step="0.05"
                  class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer 
                         [&::-webkit-slider-runnable-track]:rounded-lg [&::-webkit-slider-runnable-track]:bg-slate-200
                         [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:w-5 [&::-webkit-slider-thumb]:h-5 
                         [&::-webkit-slider-thumb]:bg-white [&::-webkit-slider-thumb]:border-2 [&::-webkit-slider-thumb]:border-indigo-600 
                         [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:shadow-md 
                         [&::-webkit-slider-thumb]:transition-transform [&::-webkit-slider-thumb]:duration-150 
                         hover:[&::-webkit-slider-thumb]:scale-110 active:[&::-webkit-slider-thumb]:scale-95 focus:outline-none"
                />
                <span class="text-[10px] font-semibold text-slate-400">45%</span>
              </div>
            </div>
          </div>
        </div>

        <div class="lg:col-span-5 lg:sticky lg:top-8">
          <div class="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm flex flex-col items-center space-y-6">
            <div class="w-full flex items-center justify-between">
              <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Live Preview</span>
              <span class="inline-flex items-center gap-1 text-[11px] font-semibold text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded-full">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                Ready
              </span>
            </div>

            <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100 flex items-center justify-center shadow-inner">
              <div ref="container" class="bg-white p-3 rounded-xl shadow-sm"></div>
            </div>

            <div class="w-full space-y-2.5">
              <button
                @click="downloadQr('png')"
                type="button"
                class="w-full flex items-center justify-center gap-2 py-3 px-4 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold rounded-xl shadow-sm hover:shadow transition-all cursor-pointer"
              >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
                </svg>
                Download PNG
              </button>

              <button
                @click="downloadQr('svg')"
                type="button"
                class="w-full flex items-center justify-center gap-2 py-3 px-4 bg-slate-800 hover:bg-slate-900 text-white text-sm font-semibold rounded-xl shadow-sm hover:shadow transition-all cursor-pointer"
              >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M9 19l3 3m0 0l3-3m-3 3V10" />
                </svg>
                Download SVG (Vector)
              </button>

              <button @click="copyToClipboard" type="button" :disabled="isCopying" class="w-full flex items-center justify-center gap-2 py-2.5 px-4 bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold rounded-xl border border-slate-200 transition-all cursor-pointer disabled:opacity-50">
                <span v-if="!copied">Copy to Clipboard</span>
                <span v-else class="text-emerald-600 font-bold">Copied!</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import QRCodeStyling from 'qr-code-styling'

const container = ref(null)
const fileInput = ref(null)
const copied = ref(false)
const isCopying = ref(false)

const defaultOptions = {
  data: 'https://google.com',
  margin: 4,
  dotsColor: '#4f46e5',
  dotsType: 'rounded',
  cornerSquareType: 'extra-rounded',
  cornerSquareColor: '#312e81',
  cornerDotType: 'dot',
  cornerDotColor: '#312e81',
  image: '',
  imageSize: 0.35
}

const options = reactive({ ...defaultOptions })

const qrCode = new QRCodeStyling({
  width: 240,
  height: 240,
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

const copyToClipboard = async () => {
  if (isCopying.value) return
  isCopying.value = true

  try {
    const blob = await qrCode.getRawData('png')
    
    if (blob) {
      await navigator.clipboard.write([
        new ClipboardItem({'image/png': blob})
      ])

      copied.value = true
      setTimeout(() => {
        copied.value = false
      }, 2000)
    }
  } catch (error) {
    console.error('Failed to copy QR code to clipboard:', error)
  }finally {
    isCopying.value = false
  }
}

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
  if (fileInput.value) {
    fileInput.value.value = ''
  }
  update()
}

const resetToDefaults = () => {
  Object.assign(options, defaultOptions)
  if (fileInput.value) {
    fileInput.value.value = ''
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