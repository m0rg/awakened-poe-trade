<template>
  <div class="max-w-md p-2">
    <div class="bg-gray-700 rounded px-2 py-1 mb-2 leading-none">
      <i class="fas fa-info-circle"></i> {{ $t('You can disable any hotkey by pressing Backspace') }}</div>
    <div class="mb-8">
      <div class="flex">
        <div class="flex-1">{{ $t('Price check') }}</div>
        <div class="flex">
          <span class="text-gray-500 mr-2">{{ $t('Auto-hide Mode') }}</span>
          <button :class="{ border: config.priceCheckKeyHold === 'Ctrl' }" @click="config.priceCheckKeyHold = 'Ctrl'; config.priceCheckKey = null" class="rounded px-1 bg-gray-900 leading-none mr-1">Ctrl</button>
          <button :class="{ border: config.priceCheckKeyHold === 'Alt' }" @click="config.priceCheckKeyHold = 'Alt'; config.priceCheckKey = null" class="rounded px-1 bg-gray-900 leading-none">Alt</button>
          <span class="mx-4">+</span>
          <hotkey-input v-model="config.priceCheckKey" :forbidden="['Ctrl','Shift','Alt', ...(config.priceCheckKeyHold === 'Ctrl' ? ['C','V','A','F'] : [])]" class="w-20" />
        </div>
      </div>
      <div class="text-right mt-2">
        <span class="text-gray-500 mr-2">{{ $t('Open without auto-hide') }}</span>
        <hotkey-input v-model="config.priceCheckLocked" class="w-48" />
      </div>
    </div>
    <div class="mb-4">
      <div class="flex">
        <div class="flex-1">{{ $t('Overlay') }} <span class="text-red-500 text-lg leading-none">*</span></div>
        <hotkey-input required v-model="config.overlayKey" class="w-48" />
      </div>
    </div>
    <div class="mb-4">
      <div class="flex">
        <div class="flex-1">{{ $t('Open item on wiki') }}</div>
        <hotkey-input v-model="config.wikiKey" class="w-48" />
      </div>
    </div>
    <div class="mb-8">
      <div class="flex">
        <div class="flex-1">{{ $t('Map check') }}</div>
        <hotkey-input v-model="config.mapCheckKey" class="w-48" />
      </div>
    </div>
    <div class="mb-8">
      <div class="flex">
        <div class="flex-1">{{ $t('Stash tab scrolling') }}</div>
        <div class="flex">
          <ui-radio v-model="config.stashScroll" :value="true" class="mr-4 font-fontin-regular">Ctrl + MouseWheel</ui-radio>
          <ui-radio v-model="config.stashScroll" :value="false">{{ $t('Disabled') }}</ui-radio>
        </div>
      </div>
    </div>
    <div class="mb-2">
      <div class="flex-1 mb-1">{{ $t('Custom commands') }}</div>
      <div class="mb-4" v-for="(command, idx) in config.commands" :key="idx">
        <input v-model.trim="command.text" class="rounded bg-gray-900 px-1 block w-full mb-1 font-fontin-regular" />
        <div class="flex justify-end">
          <button @click="removeCommand(command)" class="mr-2 text-gray-500">{{ $t('Remove') }}</button>
          <hotkey-input v-model="command.hotkey" class="w-48" />
        </div>
      </div>
      <button @click="addComand" class="bg-gray-900 rounded flex items-baseline px-2 py-1 leading-none"><i class="fas fa-plus mr-1"></i> {{ $t('Add command') }}</button>
    </div>
  </div>
</template>

<script>
import HotkeyInput from './HotkeyInput'
import { Config } from '@/web/Config'

export default {
  components: { HotkeyInput },
  computed: {
    config () {
      return Config.store
    }
  },
  methods: {
    addComand () {
      this.config.commands.push({
        text: '',
        hotkey: null
      })
    },
    removeCommand (command) {
      this.config.commands = this.config.commands.filter(c => c !== command)
    }
  }
}
</script>

<i18n>
{
  "ru": {
    "You can disable any hotkey by pressing Backspace": "Вы можете отключить сочетание, нажав клавишу Backspace",
    "Price check": "Прайс-чек",
    "Auto-hide Mode": "Режим авто-скрытия",
    "Open without auto-hide": "Открыть без авто-скрытия",
    "Overlay": "Оверлей",
    "Open item on wiki": "Открыть предмет в вики",
    "Map check": "Проверка карты",
    "Stash tab scrolling": "Прокрутка вкладок тайника",
    "Custom commands": "Команды чата",
    "Add command": "Добавить команду"
  }
}
</i18n>
