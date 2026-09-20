<template>
  <div class="min-h-screen bg-slate-50 dark:bg-slate-950 py-10 px-4 sm:px-6 lg:px-8 transition-colors duration-200">
    <div class="max-w-5xl mx-auto space-y-6">
      <!-- Header -->
      <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 pb-6 border-b border-slate-200 dark:border-slate-800">
        <div>
          <h1 class="text-2xl sm:text-3xl font-extrabold text-slate-900 dark:text-white tracking-tight">QR Code Studio</h1>
          <p class="text-sm text-slate-500 dark:text-slate-400 mt-1">Design, customize, and export high-resolution QR codes</p>
        </div>

        <div class="flex items-center gap-3">
          <!-- Dark Mode Toggle Button -->
          <button
            @click="toggleDarkMode"
            type="button"
            class="p-2.5 text-slate-600 dark:text-slate-300 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl hover:bg-slate-100 dark:hover:bg-slate-700 transition-colors cursor-pointer shadow-sm"
            title="Toggle Theme"
          >
            <svg v-if="isDark" class="w-4 h-4 text-amber-400" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" clip-rule="evenodd" />
            </svg>
            <svg v-else class="w-4 h-4 text-slate-700" fill="currentColor" viewBox="0 0 20 20">
              <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
            </svg>
          </button>

          <!-- Reset Button -->
          <button
            @click="resetToDefaults"
            type="button"
            class="inline-flex items-center justify-center gap-2 px-4 py-2 text-sm font-medium text-slate-600 dark:text-slate-300 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl hover:bg-rose-50 hover:text-rose-600 dark:hover:bg-rose-950/30 dark:hover:text-rose-400 hover:border-rose-200 dark:hover:border-rose-900 transition-colors shadow-sm cursor-pointer"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
            </svg>
            Reset to Defaults
          </button>
        </div>
      </div>

      <!-- Main Layout -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
        <!-- Configuration Controls (Left Column) -->
        <div class="lg:col-span-7 space-y-5">
          <!-- Text Input -->
          <div class="bg-white dark:bg-slate-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm space-y-2">
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-400">Content</label>
            <input
              v-model="options.data"
              @input="update"
              type="text"
              placeholder="https://example.com"
              class="w-full px-4 py-2.5 text-sm bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 rounded-xl focus:bg-white dark:focus:bg-slate-800 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all text-slate-800 dark:text-slate-100"
            />
          </div>

          <!-- Body & Pattern -->
          <div class="bg-white dark:bg-slate-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800 dark:text-slate-200">Body & Pattern</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Dot Style</label>
                <select
                  v-model="options.dotsType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl focus:bg-white dark:focus:bg-slate-800 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 dark:text-slate-200 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dots">Dots</option>
                  <option value="rounded">Rounded</option>
                  <option value="extra-rounded">Extra Rounded</option>
                  <option value="classy">Classy</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Pattern Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl">
                  <input
                    v-model="options.dotsColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 dark:text-slate-300 uppercase">{{ options.dotsColor }}</span>
                </div>
              </div>
            </div>

            <!-- Custom Margin Slider -->
            <div class="bg-slate-50/80 dark:bg-slate-800/50 p-3.5 rounded-xl border border-slate-200/80 dark:border-slate-700/80 space-y-2.5">
              <div class="flex justify-between items-center text-xs">
                <span class="font-medium text-slate-700 dark:text-slate-300">Margin Padding</span>
                <span class="px-2.5 py-0.5 font-mono text-xs font-bold text-indigo-700 dark:text-indigo-400 bg-indigo-50 dark:bg-indigo-950/50 border border-indigo-100 dark:border-indigo-900 rounded-full">
                  {{ options.margin }}px
                </span>
              </div>
              <div class="flex items-center gap-3">
                <span class="text-[10px] font-semibold text-slate-400 dark:text-slate-500">0</span>
                <input
                  v-model.number="options.margin"
                  @input="update"
                  type="range"
                  min="0"
                  max="30"
                  step="2"
                  class="w-full h-2 bg-slate-200 dark:bg-slate-700 rounded-lg appearance-none cursor-pointer 
                         [&::-webkit-slider-runnable-track]:rounded-lg [&::-webkit-slider-runnable-track]:bg-slate-200 dark:[&::-webkit-slider-runnable-track]:bg-slate-700
                         [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:w-5 [&::-webkit-slider-thumb]:h-5 
                         [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-slate-100 [&::-webkit-slider-thumb]:border-2 [&::-webkit-slider-thumb]:border-indigo-600 
                         [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:shadow-md 
                         [&::-webkit-slider-thumb]:transition-transform [&::-webkit-slider-thumb]:duration-150 
                         hover:[&::-webkit-slider-thumb]:scale-110 active:[&::-webkit-slider-thumb]:scale-95 focus:outline-none"
                />
                <span class="text-[10px] font-semibold text-slate-400 dark:text-slate-500">30</span>
              </div>
            </div>
          </div>

          <!-- Corners Styling -->
          <div class="bg-white dark:bg-slate-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800 dark:text-slate-200">Corners Styling</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Outer Corner Box</label>
                <select
                  v-model="options.cornerSquareType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl focus:bg-white dark:focus:bg-slate-800 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 dark:text-slate-200 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dot">Dot</option>
                  <option value="extra-rounded">Extra Rounded</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Outer Corner Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl">
                  <input
                    v-model="options.cornerSquareColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 dark:text-slate-300 uppercase">{{ options.cornerSquareColor }}</span>
                </div>
              </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Inner Corner Dot</label>
                <select
                  v-model="options.cornerDotType"
                  @change="update"
                  class="w-full px-3 py-2 text-sm bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl focus:bg-white dark:focus:bg-slate-800 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700 dark:text-slate-200 cursor-pointer"
                >
                  <option value="square">Square</option>
                  <option value="dot">Dot</option>
                </select>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 dark:text-slate-400 mb-1.5">Inner Corner Color</label>
                <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl">
                  <input
                    v-model="options.cornerDotColor"
                    @input="update"
                    type="color"
                    class="w-7 h-7 rounded-lg border-0 cursor-pointer bg-transparent"
                  />
                  <span class="text-xs font-mono text-slate-600 dark:text-slate-300 uppercase">{{ options.cornerDotColor }}</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Center Logo -->
          <div class="bg-white dark:bg-slate-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm space-y-4">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-indigo-500"></span>
              <h3 class="text-sm font-bold text-slate-800 dark:text-slate-200">Center Logo</h3>
            </div>

            <div>
              <input
                ref="fileInput"
                type="file"
                accept="image/*"
                @change="onLogoSelected"
                class="w-full text-xs text-slate-500 dark:text-slate-400 file:mr-4 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-indigo-50 dark:file:bg-indigo-950/60 file:text-indigo-700 dark:file:text-indigo-300 hover:file:bg-indigo-100 dark:hover:file:bg-indigo-900 cursor-pointer transition-all"
              />
            </div>

            <!-- Custom Logo Slider -->
            <div v-if="options.image" class="pt-3 border-t border-slate-100 dark:border-slate-800 space-y-3">
              <div class="flex justify-between items-center text-xs">
                <span class="font-medium text-slate-700 dark:text-slate-300">Logo Scale</span>
                <div class="flex items-center gap-2">
                  <span class="px-2.5 py-0.5 font-mono text-xs font-bold text-indigo-700 dark:text-indigo-400 bg-indigo-50 dark:bg-indigo-950/50 border border-indigo-100 dark:border-indigo-900 rounded-full">
                    {{ Math.round(options.imageSize * 100) }}%
                  </span>
                  <button
                    @click="removeLogo"
                    type="button"
                    class="text-xs font-semibold text-rose-500 hover:text-rose-700 dark:hover:text-rose-400 hover:bg-rose-50 dark:hover:bg-rose-950/40 px-2 py-0.5 rounded-md transition cursor-pointer"
                  >
                    Remove
                  </button>
                </div>
              </div>

              <div class="flex items-center gap-3">
                <span class="text-[10px] font-semibold text-slate-400 dark:text-slate-500">10%</span>
                <input
                  v-model.number="options.imageSize"
                  @input="update"
                  type="range"
                  min="0.1"
                  max="0.45"
                  step="0.05"
                  class="w-full h-2 bg-slate-200 dark:bg-slate-700 rounded-lg appearance-none cursor-pointer 
                         [&::-webkit-slider-runnable-track]:rounded-lg [&::-webkit-slider-runnable-track]:bg-slate-200 dark:[&::-webkit-slider-runnable-track]:bg-slate-700
                         [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:w-5 [&::-webkit-slider-thumb]:h-5 
                         [&::-webkit-slider-thumb]:bg-white dark:[&::-webkit-slider-thumb]:bg-slate-100 [&::-webkit-slider-thumb]:border-2 [&::-webkit-slider-thumb]:border-indigo-600 
                         [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:shadow-md 
                         [&::-webkit-slider-thumb]:transition-transform [&::-webkit-slider-thumb]:duration-150 
                         hover:[&::-webkit-slider-thumb]:scale-110 active:[&::-webkit-slider-thumb]:scale-95 focus:outline-none"
                />
                <span class="text-[10px] font-semibold text-slate-400 dark:text-slate-500">45%</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Sticky Preview & Action Card (Right Column) -->
        <div class="lg:col-span-5 lg:sticky lg:top-8">
          <div class="bg-white dark:bg-slate-900 p-6 rounded-3xl border border-slate-200 dark:border-slate-800 shadow-sm flex flex-col items-center space-y-6">
            <div class="w-full flex items-center justify-between">
              <span class="text-xs font-bold uppercase tracking-wider text-slate-400 dark:text-slate-500">Live Preview</span>
              <span class="inline-flex items-center gap-1 text-[11px] font-semibold text-emerald-700 dark:text-emerald-400 bg-emerald-50 dark:bg-emerald-950/40 px-2 py-0.5 rounded-full">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                Ready
              </span>
            </div>

            <!-- Canvas Container (سفید نگه داشته شده برای حفظ اسکن‌پذیری) -->
            <div class="p-4 bg-slate-50 dark:bg-slate-800/60 rounded-2xl border border-slate-100 dark:border-slate-800 flex items-center justify-center shadow-inner">
              <div ref="container" class="bg-white p-3 rounded-xl shadow-sm"></div>
            </div>

            <!-- Actions -->
            <div class="w-full space-y-2.5">
              <!-- Copy to Clipboard Button -->
              <button
                @click="copyToClipboard"
                type="button"
                :disabled="isCopying"
                class="w-full flex items-center justify-center gap-2 py-2.5 px-4 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 text-xs font-semibold rounded-xl border border-slate-200 dark:border-slate-700 transition-all cursor-pointer disabled:opacity-50"
              >
                <svg v-if="!copied" class="w-4 h-4 text-slate-500 dark:text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3" />
                </svg>
                <svg v-else class="w-4 h-4 text-emerald-600 dark:text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                </svg>
                <span :class="{ 'text-emerald-700 dark:text-emerald-400 font-bold': copied }">
                  {{ copied ? 'Copied to Clipboard!' : 'Copy to Clipboard' }}
                </span>
              </button>

              <div class="flex gap-2 w-full">
                <!-- PNG Download -->
                <button
                  @click="downloadQr('png')"
                  type="button"
                  class="flex-1 flex items-center justify-center gap-2 py-3 px-3 bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-semibold rounded-xl shadow-sm hover:shadow transition-all cursor-pointer"
                >
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
                  </svg>
                  PNG
                </button>

                <!-- SVG Download -->
                <button
                  @click="downloadQr('svg')"
                  type="button"
                  class="flex-1 flex items-center justify-center gap-2 py-3 px-3 bg-slate-800 hover:bg-slate-900 dark:bg-slate-700 dark:hover:bg-slate-600 text-white text-xs font-semibold rounded-xl shadow-sm hover:shadow transition-all cursor-pointer"
                >
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M9 19l3 3m0 0l3-3m-3 3V10" />
                  </svg>
                  SVG (Vector)
                </button>
              </div>
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
const isDark = ref(false)
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

onMounted(() => {
  if (container.value) {
    qrCode.append(container.value)
  }

  // مقداردهی اولیه تم از لوکال استوریج یا تنظیمات سیستم
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark' || (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  } else {
    isDark.value = false
    document.documentElement.classList.remove('dark')
  }
})

const toggleDarkMode = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
    localStorage.setItem('theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark')
    localStorage.setItem('theme', 'light')
  }
}

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

const copyToClipboard = async () => {
  if (isCopying.value) return
  isCopying.value = true

  try {
    const blob = await qrCode.getRawData('png')
    if (blob) {
      await navigator.clipboard.write([
        new ClipboardItem({ 'image/png': blob })
      ])
      copied.value = true
      setTimeout(() => {
        copied.value = false
      }, 2000)
    }
  } catch (error) {
    console.error('Failed to copy to clipboard:', error)
  } finally {
    isCopying.value = false
  }
}
</script>