<script setup lang="ts">
import { useUiStore } from "@/stores/ui"
import { useRoute } from "nuxt/app"

import Pc from "@/components/header/pc.vue"
// TODO: mobile과 siteMap 해야됩
import Mobile from "@/components/header/mobile.vue"

const store = useUiStore() // ui store
const route = useRoute() // 라우트 감지용

useHeadSafe({
  // '/' 라면 HOME을 반환하고, 아니라면 / 앞의 문자열만 대문자로 반환
  title: route.path === "/" ? "HOME" : route.path.split("/")[1].toUpperCase(),
})

const color = computed(() => {
  // header의 로고 이미지 색상
  if (route.path == "/") {
    // index 페이지만 변경
    const checkIndex = [1, 3, 4, 5]
    if (checkIndex.includes(store.landing.scrollIndex)) {
      return "black"
    }
  }
  return "white"
})
</script>

<template>
  <header
    class="gnb w-full pb-16 flex justify-center top-0 left-0 z-40 duration-500 2xl:pt-10"
    :class="[
      {
        open: store.header.nav,
        'opacity-0': store.header.hidden,
      },
      route.path == '/' ? 'fixed' : 'absolute',
    ]"
  >
    <div
      class="container lg:max-w-[90%] flex gap-5 relative 2xl:items-start items-center 2xl:justify-start justify-between"
    >
      <a class="flex w-96" href="/">
        <Transition name="header-small">
          <figure v-show="!store.header.nav" class="py-6 px-8 cursor-pointer">
            <ClientOnly>
              <img
                v-show="color == 'white'"
                class="w-72"
                src="/img/logo_header_w.png"
              />
              <img
                v-show="color == 'black'"
                class="w-72"
                src="/img/logo_header_b.png"
              />
            </ClientOnly>
          </figure>
        </Transition>
        <Transition name="header-large">
          <figure v-show="store.header.nav" class="flex py-8 items-center">
            <ClientOnly>
              <img class="w-72" src="/img/logo_header_large_b.png" />
            </ClientOnly>
          </figure>
        </Transition>
      </a>

      <Pc class="2xl:flex hidden" :color="color"></Pc>

      <Mobile class="2xl:hidden flex"></Mobile>
    </div>
  </header>
</template>
