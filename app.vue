<template>
  <div style="width: 100%">
    <div class="theme-dock">
      <div class="theme-selector">
        <div class="theme-item" v-for="(theme, name) in themes">
          <button
            class="theme-btn"
            :class="name == curTheme ? 'theme-selected' : ''"
            @click="setTheme(name)"
          >
            {{ getCapitalized(name) }}
          </button>
        </div>
      </div>
    </div>
    <div class="color-wrap">
      <div class="color-container" v-for="(clr, name) in themes[curTheme]">
        <div class="color-display" :style="{ backgroundColor: clr }">
          <div class="overlay">
            <div
              class="overlay-text"
              style="font-family: 'Consolas'; font-size: 30px; cursor: pointer"
              @click="
                (event) => {
                  copyLarge(clr, event);
                }
              "
            >
              {{ clr }}
            </div>
          </div>
        </div>
        <div class="color-text">
          {{ name }}
        </div>
        <div class="sep"></div>
        <div
          class="color-text code"
          @click="
            (event) => {
              copyCode(clr, event);
            }
          "
        >
          {{ clr }}
          <div class="overlay"><div class="overlay-text">Copied!</div></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css?family=Proza+Libre|Fira+Mono');

.theme-selected {
  background-color: #2b7dc1 !important;
}

.theme-dock {
  display: flex;
  justify-content: center;
  height: 60px;
}

.theme-selector {
  display: flex;
  margin: 10px;
  justify-content: center;
  border: 2px solid white;
  border-radius: 10px;
  overflow: hidden;
}

.theme-btn {
  height: 100%;
  background-color: transparent;
  color: white;
  border: none;
  padding-left: 20px;
  padding-right: 20px;
  cursor: pointer;
  transition-duration: 0.4s;
}
/* animation: ripple 1250ms ease-out forwards, fade 1500ms ease-out forwards; */
.theme-btn:hover {
  background-color: #2c5372;
  transition-duration: 0.4s;
}

@keyframes ripple {
  0% {
    transform: translate(-100%, -100%);
  }
  80% {
    transform: translate(-100%, -100%) scale(50);
  }
  100% {
    transform: translate(-100%, -100%) scale(50);
    opacity: 0;
  }
}

@keyframes fade {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.theme-item + .theme-item {
  border-left: 1px white solid;
}

html {
  background-color: #30363f;
  font-family: 'Proza Libre', sans-serif;
}

.color-container {
  display: flex;
  height: 250px;
  width: 200px;
  margin: 10px;
  justify-content: start;
  flex-direction: column;
  border-radius: 20px 20px 0 0;
  flex-wrap: wrap;
  box-shadow: 0 8px 10px 1px rgba(0, 0, 0, 0.2),
    0 3px 14px 2px rgba(0, 0, 0, 0.14), 0 5px 5px -3px rgba(0, 0, 0, 0.12);
  overflow: hidden;
}

.color-wrap {
  display: flex;
  justify-self: center;
  justify-content: space-between;
  align-content: space-evenly;
  flex-wrap: wrap;
  flex-basis: 90%;
}

.color-display {
  display: flex;
  flex: 0 0 70%;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.color-display .overlay {
  background-color: rgba(0, 0, 0, 0.2);
}

.color-display:hover .overlay {
  height: 100%;
}

.color-text {
  display: flex;
  flex: 0 0 15%;
  justify-content: center;
  align-content: center;
  flex-wrap: wrap;
  color: white;
  text-align: center;
  vertical-align: middle;
}

.code {
  font-family: 'Consolas' !important;
  background-color: #282c34;
  cursor: pointer;
  overflow: hidden;
  position: relative;
}

.overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #41a7fc;
  overflow: hidden;
  width: 100%;
  height: 0;
  transition: 0.2s ease;
}

.overlay-text {
  color: white;
  position: absolute;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  text-align: center;
}

.overlay-show {
  height: 100%;
}
</style>

<script setup lang="ts">
function copyLarge(str: string, event: MouseEvent) {
  (event.target as HTMLElement).textContent = 'Copied!';
  setTimeout(() => {
    (event.target as HTMLElement).textContent = str;
  }, 700);
  navigator.clipboard.writeText(str);
}

function copyCode(str: string, event: MouseEvent) {
  (event.target as HTMLElement)
    .querySelector('.overlay')
    ?.classList.add('overlay-show');

  setTimeout(() => {
    (event.target as HTMLElement)
      .querySelector('.overlay')
      ?.classList.remove('overlay-show');
  }, 500);

  navigator.clipboard.writeText(str);
}

const themes = ref({
  dark: {
    black: '#181a1f',
    bg0: '#282c34',
    bg1: '#31353f',
    bg2: '#393f4a',
    bg3: '#3b3f4c',
    bg_d: '#21252b',
    bg_blue: '#73b8f1',
    bg_yellow: '#ebd09c',
    fg: '#abb2bf',
    purple: '#c678dd',
    green: '#98c379',
    orange: '#d19a66',
    blue: '#61afef',
    yellow: '#e5c07b',
    cyan: '#56b6c2',
    red: '#e86671',
    grey: '#5c6370',
    light_grey: '#848b98',
    dark_cyan: '#2b6f77',
    dark_red: '#993939',
    dark_yellow: '#93691d',
    dark_purple: '#8a3fa0',
    diff_add: '#31392b',
    diff_delete: '#382b2c',
    diff_change: '#1c3448',
    diff_text: '#2c5372',
  },
  darker: {
    black: '#0e1013',
    bg0: '#1f2329',
    bg1: '#282c34',
    bg2: '#30363f',
    bg3: '#323641',
    bg_d: '#181b20',
    bg_blue: '#61afef',
    bg_yellow: '#e8c88c',
    fg: '#a0a8b7',
    purple: '#bf68d9',
    green: '#8ebd6b',
    orange: '#cc9057',
    blue: '#4fa6ed',
    yellow: '#e2b86b',
    cyan: '#48b0bd',
    red: '#e55561',
    grey: '#535965',
    light_grey: '#7a818e',
    dark_cyan: '#266269',
    dark_red: '#8b3434',
    dark_yellow: '#835d1a',
    dark_purple: '#7e3992',
    diff_add: '#272e23',
    diff_delete: '#2d2223',
    diff_change: '#172a3a',
    diff_text: '#274964',
  },
  cool: {
    black: '#151820',
    bg0: '#242b38',
    bg1: '#2d3343',
    bg2: '#343e4f',
    bg3: '#363c51',
    bg_d: '#1e242e',
    bg_blue: '#6db9f7',
    bg_yellow: '#f0d197',
    fg: '#a5b0c5',
    purple: '#ca72e4',
    green: '#97ca72',
    orange: '#d99a5e',
    blue: '#5ab0f6',
    yellow: '#ebc275',
    cyan: '#4dbdcb',
    red: '#ef5f6b',
    grey: '#546178',
    light_grey: '#7d899f',
    dark_cyan: '#25747d',
    dark_red: '#a13131',
    dark_yellow: '#9a6b16',
    dark_purple: '#8f36a9',
    diff_add: '#303d27',
    diff_delete: '#3c2729',
    diff_change: '#18344c',
    diff_text: '#265478',
  },
  deep: {
    black: '#0c0e15',
    bg0: '#1a212e',
    bg1: '#21283b',
    bg2: '#283347',
    bg3: '#2a324a',
    bg_d: '#141b24',
    bg_blue: '#54b0fd',
    bg_yellow: '#f2cc81',
    fg: '#93a4c3',
    purple: '#c75ae8',
    green: '#8bcd5b',
    orange: '#dd9046',
    blue: '#41a7fc',
    yellow: '#efbd5d',
    cyan: '#34bfd0',
    red: '#f65866',
    grey: '#455574',
    light_grey: '#6c7d9c',
    dark_cyan: '#1b6a73',
    dark_red: '#992525',
    dark_yellow: '#8f610d',
    dark_purple: '#862aa1',
    diff_add: '#27341c',
    diff_delete: '#331c1e',
    diff_change: '#102b40',
    diff_text: '#1c4a6e',
  },
  warm: {
    black: '#191a1c',
    bg0: '#2c2d30',
    bg1: '#35373b',
    bg2: '#3e4045',
    bg3: '#404247',
    bg_d: '#242628',
    bg_blue: '#79b7eb',
    bg_yellow: '#e6cfa1',
    fg: '#b1b4b9',
    purple: '#c27fd7',
    green: '#99bc80',
    orange: '#c99a6e',
    blue: '#68aee8',
    yellow: '#dfbe81',
    cyan: '#5fafb9',
    red: '#e16d77',
    grey: '#646568',
    light_grey: '#8b8d91',
    dark_cyan: '#316a71',
    dark_red: '#914141',
    dark_yellow: '#8c6724',
    dark_purple: '#854897',
    diff_add: '#32352f',
    diff_delete: '#342f2f',
    diff_change: '#203444',
    diff_text: '#32526c',
  },
  warmer: {
    black: '#101012',
    bg0: '#232326',
    bg1: '#2c2d31',
    bg2: '#35363b',
    bg3: '#37383d',
    bg_d: '#1b1c1e',
    bg_blue: '#68aee8',
    bg_yellow: '#e2c792',
    fg: '#a7aab0',
    purple: '#bb70d2',
    green: '#8fb573',
    orange: '#c49060',
    blue: '#57a5e5',
    yellow: '#dbb671',
    cyan: '#51a8b3',
    red: '#de5d68',
    grey: '#5a5b5e',
    light_grey: '#818387',
    dark_cyan: '#2b5d63',
    dark_red: '#833b3b',
    dark_yellow: '#7c5c20',
    dark_purple: '#79428a',
    diff_add: '#282b26',
    diff_delete: '#2a2626',
    diff_change: '#1a2a37',
    diff_text: '#2c485f',
  },
  light: {
    black: '#101012',
    bg0: '#fafafa',
    bg1: '#f0f0f0',
    bg2: '#e6e6e6',
    bg3: '#dcdcdc',
    bg_d: '#c9c9c9',
    bg_blue: '#68aee8',
    bg_yellow: '#e2c792',
    fg: '#383a42',
    purple: '#a626a4',
    green: '#50a14f',
    orange: '#c18401',
    blue: '#4078f2',
    yellow: '#986801',
    cyan: '#0184bc',
    red: '#e45649',
    grey: '#a0a1a7',
    light_grey: '#818387',
    dark_cyan: '#2b5d63',
    dark_red: '#833b3b',
    dark_yellow: '#7c5c20',
    dark_purple: '#79428a',
    diff_add: '#e2fbe4',
    diff_delete: '#fce2e5',
    diff_change: '#e2ecfb',
    diff_text: '#cad3e0',
  },
});

const curTheme = ref<keyof typeof themes.value>('dark');

function setTheme(name: keyof typeof themes.value) {
  curTheme.value = name;
}

function getCapitalized(name: string) {
  return name[0].toUpperCase() + name.slice(1);
}

const darker: object = ref({
  black: '#0e1013',
  bg0: '#1f2329',
  bg1: '#282c34',
  bg2: '#30363f',
  bg3: '#323641',
  bg_d: '#181b20',
  bg_blue: '#61afef',
  bg_yellow: '#e8c88c',
  fg: '#a0a8b7',
  purple: '#bf68d9',
  green: '#8ebd6b',
  orange: '#cc9057',
  blue: '#4fa6ed',
  yellow: '#e2b86b',
  cyan: '#48b0bd',
  red: '#e55561',
  grey: '#535965',
  light_grey: '#7a818e',
  dark_cyan: '#266269',
  dark_red: '#8b3434',
  dark_yellow: '#835d1a',
  dark_purple: '#7e3992',
  diff_add: '#272e23',
  diff_delete: '#2d2223',
  diff_change: '#172a3a',
  diff_text: '#274964',
});
</script>
