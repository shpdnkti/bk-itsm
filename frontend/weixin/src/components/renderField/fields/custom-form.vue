<!--
  - Tencent is pleased to support the open source community by making BK-ITSM 蓝鲸流程服务 available.
  - Copyright (C) 2021 THL A29 Limited, a Tencent company.  All rights reserved.
  - BK-ITSM 蓝鲸流程服务 is licensed under the MIT License.
  -
  - License for BK-ITSM 蓝鲸流程服务:
  - -------------------------------------------------------------------
  -
  - Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
  - documentation files (the "Software"), to deal in the Software without restriction, including without limitation
  - the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software,
  - and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
  - The above copyright notice and this permission notice shall be included in all copies or substantial
  - portions of the Software.
  -
  - THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT
  - LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN
  - NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
  - WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
  - SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE
  -->

<template>
  <p class="common-view-field" style="display: block">
    <label class="view-label">{{ item.name }}</label>
    <renderView :form-data="formData" :context="context" />
  </p>
</template>
<script lang="ts">
import { defineComponent, toRefs, computed } from 'vue'
import renderView from '@/components/renderView'
// import { CUSTOM_FORM_TEMPLATE } from '@/components/renderView/template'

export default defineComponent({
  name: 'FieldCustomForm',
  components: {
    renderView
  },
  props: {
    item: {
      type: Object,
      default: () => ({})
    }
  },
  setup(props) {
    const { item } =  toRefs<{ item: any }>(props)
    const customForm = item.value.value
    const formData = computed(() => customForm.form_data)
    const context = computed(() => ({
      config: customForm.config || {},
      schemes: customForm.schemes
    }))
    return {
      formData,
      context
    }
    return {
      // eslint-disable-next-line vue/no-dupe-keys
      item
    }
  }
})
</script>
<style lang="postcss" scoped>
  /deep/ .van-cell__title {
    width: 100%;
    .van-cell__label {
      padding: 10px 20px;
    }
  }
</style>
