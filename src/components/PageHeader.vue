<template>
  <div class="header clearfix">
    <router-link to="/" class="logo-box fl">
      <img class="logo" :src="headerLogo" alt="logo" />
      <div class="logo-text">{{ logoText }}</div>
    </router-link>
    <div class="fr clearfix">
      <el-autocomplete
        v-model="searchValue"
        class="search-box fl"
        :fetch-suggestions="querySearchAsync"
        placeholder="请输入内容"
        clearable
        size="default"
        :prefix-icon="Search"
        popper-class="autocomplete-popper"
        @select="searchHandleSelect"
      >
        <template #default="{ item }">
          <div class="option-item-box">
            <div class="name ellipsis">{{ getSearchName(item.type) }}</div>
            <div class="content ellipsis">{{ item.articleTitle }}</div>
          </div>
        </template>
      </el-autocomplete>
      <el-menu
        class="header-nav fl"
        :default-active="activeIndex"
        mode="horizontal"
        :router="true"
        :ellipsis="false"
      >
        <el-menu-item
          v-for="item in menuObj"
          :key="item.index"
          :index="item.index"
          :route="item.route"
        >
          {{ item.title }}
        </el-menu-item>
      </el-menu>
    </div>
  </div>
</template>

<script setup>
  import headerLogo from '@/assets/img/headerLogo.jpg'
  import { Search } from '@element-plus/icons-vue'
  import { ref, computed } from 'vue'
  import { useStore } from 'vuex'
  import { useRouter, useRoute } from 'vue-router'
  import { apiSearch } from '@/service/basic.js'

  const router = useRouter()
  const route = useRoute()
  const store = useStore()
  const logoText = store.state.logoText

  // 搜索
  const searchValue = ref('')
  async function querySearchAsync(queryString, cb) {
    if (!queryString.length) return
    const result = await apiSearch({
      queryString
    })
    cb(result?.data?.searchList || [])
  }
  function searchHandleSelect(key) {
    if ([0, 7].includes(key.type)) {
      window.open(`${import.meta.env.VITE_detailPath}/detail/${key.articleId}`)
    } else if ([1, 2, 3, 4, 5, 6].includes(key.type)) {
      window.open(
        `${import.meta.env.VITE_detailPath}/detail/${key.articleId}#${
          key[`h${key.type}`]
        }`
      )
    }
  }
  function getSearchName(type) {
    let res
    switch (type) {
      case 0:
        res = '文章标题'
        break
      case 1:
        res = '一级标题'
        break
      case 2:
        res = '二级标题'
        break
      case 3:
        res = '三级标题'
        break
      case 4:
        res = '四级标题'
        break
      case 5:
        res = '五级标题'
        break
      case 6:
        res = '六级标题'
        break
      case 7:
        res = '关键词'
        break
      default:
        break
    }
    return res
  }

  // 菜单
  const menuObj = [
    {
      index: '1',
      route: '/',
      title: '首页'
    },
    {
      index: '2',
      route: '/list',
      title: '归档'
    },
    {
      index: '3',
      route: '/about',
      title: '关于'
    }
  ]
  const activeIndex = computed(() => {
    let res
    switch (route.name) {
      case 'home':
        res = '1'
        break
      case 'list':
        res = '2'
        break
      case 'about':
        res = '3'
        break
      default:
        res = '1'
        break
    }
    return res
  })
</script>

<style lang="scss" scoped>
  @import './../styles/components/page-header.scss';
</style>

<style lang="scss">
  .search-box {
    margin: 14px 30px;
  }
  .autocomplete-popper {
    width: 600px !important;
    .el-autocomplete-suggestion__wrap {
      padding: 10px;
      max-height: none;
    }
    li {
      padding: 0 10px;
      border-left: 1px solid $border-color;
      border-right: 1px solid $border-color;
      border-bottom: 1px solid $border-color;
      &:nth-child(1) {
        border-top: 1px solid $border-color;
      }
      .option-item-box {
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: left;
        .name {
          flex: 0 0 15%;
          max-width: 15%;
        }
        .content {
          box-sizing: border-box;
          padding-left: 10px;
          flex: 0 0 85%;
          max-width: 85%;
          border-left: 1px solid $border-color;
        }
      }
    }
  }
</style>
