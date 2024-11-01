<template>
  <div class="catalog">
    <select v-model="currentLang" @change="isFirstLevelExpanded = [false]">
      <option value="ru">Русский</option>
      <option value="en">English</option>
      <option value="fr">Français</option>
    </select>

    <div class="catalog__items">
      <div
        v-for="(item, index) in data"
        :key="item.id"
        :class="{ catalog__item: true, 'catalog__item--expanded': isFirstLevelExpanded[index] }"
      >
        <h1
          @click="isFirstLevelExpanded[index] = !isFirstLevelExpanded[index]"
          class="catalog__item-title"
        >
          {{ item.locale[currentLang].cg_name || item.locale['ru'].cg_name }}
        </h1>

        <div class="catalog__item-description">
          {{ item.locale[currentLang].cg_description || item.locale['ru'].cg_description }}
        </div>

        <div v-if="item.childs && isFirstLevelExpanded[index]" class="catalog__item-children">
          <div v-for="child in item.childs" :key="child.id">
            <a
              :href="
                child.locale[currentLang].link + '' + child.locale[currentLang].id ||
                child.locale['ru'].link + '' + child.locale['ru'].id
              "
              class="catalog__item-child"
              target="_blank"
            >
              {{ child.locale[currentLang].cg_name || child.locale['ru'].cg_name }}
            </a>

            <div class="catalog__item-breadcrumbs">
              {{ item.locale[currentLang].cg_name || item.locale['ru'].cg_name }} ->
              {{ child.locale[currentLang].cg_name || child.locale['ru'].cg_name }}
            </div>

            <div class="catalog__item-children-description">
              {{ child.locale[currentLang].cg_description || child.locale['ru'].cg_description }}
            </div>
          </div>
        </div>

        <div
          v-if="!item.childs && isFirstLevelExpanded[index]"
          class="catalog__item-children catalog__item-children--empty"
        >
          Нет содержимого
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// Vue imports
import { ref } from 'vue';

// Data
import dataJson from './mock/task_json.json';

//Interfaces
interface IData {
  id: number;
  depth: number;
  numchild: number;
  category_image: string | null;
  logo_image: string | null;
  menu_image: string | null;
  separate_menu: boolean;
  c_images: number;
  product_rep_1_id: number | null;
  product_rep_2_id: number | null;
  c_views: number;
  og: null;
  twitter: null;
  locale: { [lang: string]: ICategoryLocale };
  search_target: boolean;
  path_to_top: number[];
  childs: IChilds[];
}

interface ICategoryLocale {
  id?: number;
  cg_name?: string;
  cg_description?: string;
  cg_title?: string;
  cg_slug?: string;
  meta_description?: string;
  meta_keywords?: string;
  link?: string;
}

interface IChilds {
  id: number;
  depth: number;
  numchild: number;
  category_image: string | null;
  logo_image: string | null;
  menu_image: string | null;
  separate_menu: boolean;
  c_images: number;
  product_rep_1_id: number | null;
  product_rep_2_id: number | null;
  c_views: number;
  og: IChildrenOg[];
  twitter: IChildrenTwitter[];
  locale: { [lang: string]: IChildrenLocale };
  path_to_top: number[];
}

interface IChildrenOg {
  id: number;
  cat_id: number;
  og_property_name: string;
  locale: { [lang: string]: object };
}

interface IChildrenTwitter {
  id: number;
  cat_id: number;
  extra_meta_name: string;
  locale: { [lang: string]: object };
}

interface IChildrenLocale {
  id?: number;
  cg_name?: string;
  cg_description?: string;
  cg_title?: string;
  cg_slug?: string;
  meta_description?: string;
  meta_keywords?: string;
  link?: string;
}

const data = ref<IData[]>(dataJson as IData[]);

const currentLang = ref('ru');
const isFirstLevelExpanded = ref([false]);
</script>

<style lang="scss" scoped>
.catalog {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  text-align: left;
  gap: 40px;
  width: 100%;

  &__items {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    text-align: left;
    gap: 20px;
    width: 100%;
  }

  &__item {
    border: 1px solid #ccc;
    width: 100%;
    padding: 15px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    gap: 10px;

    &-title {
      cursor: pointer;

      &:hover {
        text-decoration: underline;
      }
    }

    &--expanded {
      border: 1px solid #dedede;
      width: 100%;
      padding: 15px;
    }

    &-children {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      justify-content: center;
      text-align: left;
      gap: 20px;

      &--empty {
        font-size: 12px;
        color: red;
      }

      &-description {
        font-size: 12px;
      }
    }
  }
}
</style>
