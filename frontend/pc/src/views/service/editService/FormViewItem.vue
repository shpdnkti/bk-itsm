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
    <div class="form-view-item">
        <div class="form-view-content">
            <component
                :is="'CW-' + form.type"
                :item="form"
                :fields="fields">
            </component>
        </div>
        <div class="mask"></div>
        <div class="opt-btns">
            <i class="btn-item bk-itsm-icon icon-itsm-icon-three-four" @click="$emit('onFormEditClick', form)"></i>
            <i class="btn-item bk-itsm-icon icon-itsm-icon-copy" @click="$emit('onFormCloneClick', form)"></i>
            <i class="btn-item bk-icon icon-delete" :class="deleteDisabled ? 'disabled' : ''" @click="onDeleteClick(form)"></i>
        </div>
    </div>
</template>
<script>
    function registerFields () {
        const fieldComponents = {}
        const fieldFiles = require.context(
            '../../commonComponent/fieldComponent/',
            false,
            /\w+\.vue$/
        )
        fieldFiles.keys().forEach(key => {
            const componentConfig = fieldFiles(key)
            const comp = componentConfig.default
            fieldComponents[`CW-${comp.name}`] = comp
        })

        return fieldComponents
    }

    export default {
        name: 'FormViewItem',
        props: {
            fields: Array,
            form: Object
        },
        computed: {
            deleteDisabled () {
                const defaultField = ['impact', 'urgency', 'priority', 'current_status']
                return this.form.is_builtin && defaultField.indexOf(this.form.key) === -1
            }
        },
        beforeCreate () {
            const fieldComponents = registerFields()
            Object.keys(fieldComponents).forEach(name => {
                this.$options.components[name] = fieldComponents[name]
            })
        },
        methods: {
            onDeleteClick (form) {
                if (!this.deleteDisabled) {
                    this.$emit('onFormDeleteClick', form)
                }
            }
        }
    }
</script>
<style lang="scss" scoped>
    .form-view-item {
        position: relative;
        padding: 10px 0;
        overflow: hidden;
        border-top: 1px solid transparent;
        border-bottom: 1px solid transparent;
        &:hover {
            background: #fcfcfc;
            border-top: 1px solid #dde4eb;
            border-bottom: 1px solid #dde4eb;
            .mask {
                display: block;
            }
            .opt-btns {
                right: 0;
            }
        }
        .form-view-content {
            width: 600px;
            margin: 0 auto;
        }
        .mask {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            display: block;
            cursor: move;
            z-index: 1;
        }
        .opt-btns {
            position: absolute;
            right: -32px;
            top: 0;
            width: 32px;
            height: 100%;
            z-index: 2;
            border-left: 1px solid #dde4eb;
            border-right: 1px solid #dde4eb;
            background: #fcfcfc;
            text-align: center;
            transition: right .3s;
            .btn-item {
                margin-top: 8px;
                display: inline-block;
                color: #979ba5;
                cursor: pointer;
                &:not(.disabled):hover {
                    color: #63656E;
                }
                &.disabled {
                    cursor: not-allowed;
                }
            }
        }
    }
</style>
