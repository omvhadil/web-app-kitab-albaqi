<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { data } from '../constans/index.js'
import { useRoute } from 'vue-router'
import Header from '../components/Header.vue'
import { ref, computed } from 'vue'

const categoryNumber = ref(0)

const kitabId = useRoute().params.kitab
const categoryId = useRoute().params.category
const kitab = data.find((item) => item.slug === kitabId)
const category = kitab.category.find((item) => item.slug === categoryId)
const dzikri = category.dzikri

const tampilCategory = computed(() => dzikri[categoryNumber.value])

const jmlcategorydzikir = dzikri.length
const jmlname = dzikri[categoryNumber.value].name.length
const jmlSubname = dzikri[categoryNumber.value].subName.length
</script>
<template>
  <Header :title="kitab.arab">
    <template #menu>
      <div
        v-if="jmlcategorydzikir > 1"
        class="d-flex bg-gradient-first-color-alt p-2 px-3 gap-2 w-100"
      >
        <button
          v-for="item in category?.dzikri"
          :key="item.id"
          @click="categoryNumber = item.id - 1"
          class="btn bg-gradient-first-color border-light w-100 p-1 text-white"
          type="button"
        >
          {{ item.title }}
        </button>
      </div>
    </template>
  </Header>

  <div class="container mt-5 px-2" :class="jmlcategorydzikir > 1 ? 'py-5' : 'py-0'">
    <div class="pt-5">
      <div class="text-center mb-3">
        <div>
          <h4 class="m-0">﴾ {{ category.title }} ﴿</h4>
          <h4 v-if="jmlname" class="m-0">🍀 {{ tampilCategory?.name }} 🍀</h4>
          <span v-if="jmlSubname" class="m-0">{{ tampilCategory?.subName }}</span>
        </div>
      </div>
      <table class="table">
        <tbody>
          <tr v-for="item in tampilCategory?.bait_bait" :key="item.id">
            <td class="col-11" :class="item.arab ? 'border-bottom' : ''">
              <h2
                style="direction: rtl; unicode-bidi: bidi-override; line-height: 2 !important"
                class="m-0 py-1"
                align="justify"
              >
                {{ item.arab }}
              </h2>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
