<template name="ChoiceSelected">
    <!-- 自定义下拉选择框 start-->
    <view class="selected-all">
        <view :class="isShowChoice ? 'drop-down-box-selected' : 'drop-down-box'" @click="btnShowHideClick">
            <text class="dropdown-content">{undefined{choiceContent}}</text>
            <view>
                <image class="dropdown-icon" src="../static/bottom_jiantou.png" mode="widthFix"></image>
            </view>
        </view>
        <!-- 弹框内容 -->
        <view class="dialog-view" v-if="isShowChoice">
            <text :class="choiceIndex ==index ?'dialog-title-selected':'dialog-title'"
                v-for="(item ,index) in choiceList" @click="btnChoiceClick(index)">{undefined{item.choiceItemContent}}</text>
        </view>
    </view>
    <!-- 自定义下拉选择框 end -->
</template>

<script>
    export default {undefined
        name: "ChoiceSelected",
        data() {undefined
            return {undefined
                isShowChoice: false
            };
        },
        props: {undefined
            choiceIndex: {},
            choiceList: {},
            choiceContent: {}
        },
        methods: {undefined
            // 选择
            btnChoiceClick: function(position) {undefined
                var _this = this
                // _this.choiceIndex = position
                _this.isShowChoice = false
                // _this.choiceContent = _this.choiceList[position].choiceItemContent
                _this.$emit("onChoiceClick",position)
            },
            // 显示与隐藏选择内容
            btnShowHideClick: function() {undefined
                var _this = this
                if (_this.isShowChoice) {undefined
                    _this.isShowChoice = false
                } else {undefined
                    _this.isShowChoice = true
                }
            },
        }
    }
</script>

<style>
    /* end */
    .dialog-title-selected {undefined
        color: white;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 6rpx;
        padding-bottom: 6rpx;
        padding-right: 15rpx;
        background-color: #55ffff;
    }

    .dialog-title {undefined
        color: black;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 6rpx;
        padding-bottom: 6rpx;
        padding-right: 15rpx;
        background-color: white;
    }

    .dialog-view {undefined
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 100%;
        border: 2rpx solid #F0AD4E;
        box-sizing: border-box;
    }

    .dropdown-icon {undefined
        width: 30rpx;
        height: 30rpx;
        margin-left: 15rpx;
        margin-right: 20rpx;
    }

    .dropdown-content {undefined
        color: black;
        font-size: 30rpx;
        padding-left: 20rpx;
        padding-top: 5rpx;
        padding-bottom: 5rpx;
        flex-grow: 1;
    }

    .drop-down-box-selected {undefined
        display: flex;
        flex-direction: row;
        align-items: center;
        min-height: 60rpx;
        width: 100%;
        border: 2rpx solid #F0AD4E;
        box-sizing: border-box;
    }

    .drop-down-box {undefined
        display: flex;
        flex-direction: row;
        align-items: center;
        min-height: 60rpx;
        width: 100%;
        border: 2rpx solid gray;
        border-radius: 5rpx;
        box-sizing: border-box;
    }

    .selected-all {undefined
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 100%;
    }

    /* start */
</style>
————————————————
版权声明：本文为CSDN博主「yyxhzdm」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yyxhzdm/article/details/120483050