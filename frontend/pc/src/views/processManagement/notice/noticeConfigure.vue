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
    <div class="bk-itsm-service">
        <!-- title -->
        <div class="is-title" :class="{ 'bk-title-left': !sliderStatus }">
            <p class="bk-come-back">
                {{ $t(`m.navigation["通知配置"]`) }}
            </p>
        </div>
        <div class="itsm-page-content">
            <!-- tab -->
            <ul class="bk-notice-tab">
                <li v-for="(item, index) in remindWayList"
                    :key="item.id"
                    :class="{ 'bk-check-notice': checkId === item.id }"
                    @click="changeNotice(item, index)">
                    <span>{{ item.name }}</span>
                </li>
            </ul>
            <!-- content table -->
            <bk-table
                v-bkloading="{ isLoading: isDataLoading }"
                :data="noticeList"
                :size="'small'">
                <bk-table-column type="index" label="No." align="center" width="60"></bk-table-column>
                <bk-table-column :label="$t(`m.deployPage['通知类型']`)">
                    <template slot-scope="props">
                        <span class="bk-lable-primary" @click="editorInfo(props.row)">{{props.row.action_name}}</span>
                    </template>
                </bk-table-column>
                <bk-table-column :label="$t(`m.slaContent['更新时间']`)" prop="update_at"></bk-table-column>
                <bk-table-column :label="$t(`m.deployPage['更新人']`)" prop="updated_by"></bk-table-column>
                <bk-table-column :label="$t(`m.deployPage['操作']`)" width="150">
                    <template slot-scope="props">
                        <bk-button theme="primary" text @click="editorInfo(props.row)">
                            {{ $t('m.deployPage["编辑"]') }}
                        </bk-button>
                    </template>
                </bk-table-column>
            </bk-table>
        </div>
        <!-- 编辑右侧弹窗 -->
        <div class="bk-add-data">
            <bk-sideslider
                :is-show.sync="noticeInfo.show"
                :title="noticeInfo.title"
                :width="noticeInfo.width">
                <div slot="content" style="padding: 20px 34px;" v-if="noticeInfo.show">
                    <editor-notice
                        :check-id="checkId"
                        :notice-info="noticeInfo.formInfo"
                        @closeEditor="closeEditor">
                    </editor-notice>
                </div>
            </bk-sideslider>
        </div>
    </div>
</template>

<script>
    import { errorHandler } from '../../../utils/errorHandler'
    import editorNotice from './editorNotice.vue'

    export default {
        name: 'noticeConfigure',
        components: {
            editorNotice
        },
        data () {
            return {
                isDataLoading: true,
                remindWayList: [
                    { id: 'WEIXIN', name: this.$t('m.treeinfo["企业微信"]') },
                    { id: 'EMAIL', name: this.$t('m.treeinfo["邮件"]') },
                    { id: 'SMS', name: this.$t('m.treeinfo["手机短信"]') }
                ],
                checkId: 'WEIXIN',
                noticeList: [],
                noticeInfo: {
                    show: false,
                    title: this.$t('m.deployPage["编辑"]'),
                    width: 700,
                    formInfo: {}
                }
            }
        },
        computed: {
            sliderStatus () {
                return this.$store.state.common.slideStatus
            }
        },
        mounted () {
            this.getNoticeList()
        },
        methods: {
            // 获取数据
            getNoticeList () {
                this.isDataLoading = true
                const params = {
                    notify_type: this.checkId
                }
                this.$store.dispatch('noticeConfigure/getNoticeList', { params }).then((res) => {
                    this.noticeList = res.data
                }).catch((res) => {
                    errorHandler(res, this)
                }).finally(() => {
                    this.isDataLoading = false
                })
            },
            changeNotice (item, index) {
                this.checkId = item.id
                this.getNoticeList()
            },
            editorInfo (item) {
                this.noticeInfo.formInfo = item
                this.noticeInfo.show = true
            },
            closeEditor () {
                this.noticeInfo.show = false
            }
        }
    }
</script>

<style lang='scss' scoped>
    @import '../../../scss/mixins/clearfix.scss';
    @import '../../../scss/mixins/scroller.scss';

    .bk-notice-tab {
        @include clearfix;
        border-bottom: 1px solid #DDE4EB;
        margin-bottom: 10px;

        li {
            float: left;
            padding: 0 10px;
            line-height: 46px;
            text-align: center;
            color: #63656E;
            cursor: pointer;
            font-size: 14px;

            &:hover {
                color: #3A84FF;
            }
        }

        .bk-check-notice {
            border-bottom: 2px solid #3A84FF;
            color: #3A84FF;
        }
    }

    .bk-notice-table {
        margin-top: 10px;
        min-height: 200px;

        .bk-lable-primary {
            color: #3A84FF;
            cursor: pointer;
        }
    }
</style>
