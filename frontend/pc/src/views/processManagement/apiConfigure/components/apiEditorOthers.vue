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
    <div class="bk-api-editor-others">
        <bk-form
            :label-width="120"
            :model="basicInfo">
            <bk-form-item
                :label="$t(`m.systemConfig['开放接口：']`)">
                <bk-switcher v-model="basicInfo.is_activated" size="small" :disabled="basicInfo.is_builtin"></bk-switcher>
            </bk-form-item>
        </bk-form>
    </div>
</template>

<script>
    export default {
        name: 'apiEditorOthers',
        props: {
            detailInfoOri: {
                type: Object,
                default () {
                    return {}
                }
            }
        },
        data () {
            return {
                
            }
        },
        computed: {
            // 基本设置
            basicInfo: {
                // getter
                get: function () {
                    return this.detailInfoOri
                },
                // setter
                set: function (newVal) {
                    this.$parent.DetailInfo = newVal
                }
            }
        },
        methods: {
            
        }
    }
</script>

<style lang="scss" scoped>
    
</style>
