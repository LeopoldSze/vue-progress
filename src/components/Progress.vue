<template>
    <div class="progress" :class="[status ? `is-${status}` : '', `progress--${type}`]">
        <div v-if="type === 'line'" class="progress-bar">
            <div class="progress__outer" :style="{ height: strokWidth + 'px' }">
                <div v-if="textInside && showText" class="progress__inner" :style="progress__inner__style">{{ percentage }}%</div>
                <div v-else class="progress__inner" :style="progress__inner__style"></div>
            </div>
        </div>
        <div v-if="!textInside && showText" class="progress__text" :style="progress__text__style">
            <template v-if="!status">{{ percentage }}%</template>
            <i v-else class="icon" :class="icon__class"></i>
        </div>
        <div v-if="type === 'circle'" class="progress-circle" :style="{width: width + 'px', height: width + 'px'}">
            环形
            <svg viewBox="0 0 100 100">
                <path
                    :d="strokePath"
                    fill="none"
                    :strok-width="strokWidth"
                    stroke="#e5e9f2"
                />
                <path
                    :d="strokePath"
                    fill="none"
                    :strok-width="strokWidth"
                    :stroke="progressBackColor"
                    :style="circlePathStyle"
                    stroke-linecap="round"
                />
            </svg>
        </div>
    </div>
</template>

<script>
export default {
    name: "Progress",
    props: {
        strokWidth: {
            type: Number,
            default: 6
        },
        percentage: {
            type: Number,
            required: true,
            default: 0,
            validator: (value) => value >=0 && value <= 100
        },
        color: {
            type: String
        },
        status: {
            type: String
        },
        type: {
            type: String,
            default: 'line',
            validator: val => ['circle', 'line'].includes(val)
        },
        textInside: {
            type: Boolean,
            default: false
        },
        showText: {
            type: Boolean,
            default: true
        },
        width: {
            type: Number,
            default: 160
        }
    },
    data() {
        return {

        }
    },
    computed: {
        progressFontSize() {
            return 12 + this.strokWidth * 0.4 + 'px'
        },
        progressBackColor() {
            let color
            if (this.color) return this.color
            switch(this.status) {
                case 'success':
                    color = '#13ce66'
                    break
                case 'exception':
                    color = '#ff4949'
                    break
                default:
                    color = '#20a0ff'
            }
            return color
        },
        progress__inner__style() {
            return {
                width: this.percentage + '%',
                backgroundColor: this.progressBackColor,
                lineHeight: this.strokWidth + 'px',
                color: '#fff',
                textAlign: 'right',
                paddingRight: 5 + 'px'
            }
        },
        progress__text__style() {
            return {
                lineHeight: this.strokWidth + 'px',
                fontSize: this.progressFontSize
            }
        },
        icon__class() {
            if (this.type === 'line') {
                return this.status === 'success' ? 'icon-circle-check' : 'icon-circle-close'
            } else {
                return this.status === 'success' ? 'icon-check' : 'icon-close'
            }
        },
        strokePath() {
            const len = 50 - this.relativeStrokeWidth / 2
            return `
                M 50 50
                m 0 -${len}
                a ${len} ${len} 0 1 1 0 ${len * 2}
                a ${len} ${len} 0 1 1 0 -${len * 2}
            `
        },
        relativeStrokeWidth() {
            return this.strokWidth * 100 / this.width
        },
        parimeter() {
            const len = 50 - this.relativeStrokeWidth / 2
            return 2 * Math.PI * len
        },
        circlePathStyle() {
            const len  = this.parimeter
            return {
                strokeDasharray: `${len}px, ${len}px`,
                strokeDashoffset: (1 - this.percentage / 100) * len + 'px'
            }
        }
    }
}
</script>

<style scoped lang="less">
    @font-face {
        font-family: 'iconfont';  /* project id 1477867 */
        src: url('../assets/font/iconfont.eot');
        src: url('../assets/font/iconfont.eot?#iefix') format('embedded-opentype'),
        url('../assets/font/iconfont.woff2') format('woff2'),
        url('../assets/font/iconfont.woff') format('woff'),
        url('../assets/font/iconfont.ttf') format('truetype'),
        url('../assets/font/iconfont.svg#iconfont') format('svg');
    }
    .icon{
        font-family: 'iconfont',monospace !important;
        font-size: 20px;
        font-style: normal;
    }
    .icon-check::before{
        content: '\e742';
    }
    .icon-close::before{
        content: '\e62a';
    }
    .icon-circle-check::before{
        content: '\e68e';
    }
    .icon-circle-close::before{
        content: '\e630';
    }
    .progress{
        display: flex;
        margin: 20px 0;
        .progress-bar{
            width: 100%;
            margin-right: 10px;
            .progress__outer{
                background: #ebeef5;
                border-radius: 100px;
                .progress__inner{
                    height: 100%;
                    background: #f00;
                    border-radius: 100px;
                    transition: width .6s ease;
                }
            }
            .progress__text{
                color: #606266;
            }
        }
    }
    .progress.is-success{
        .progress__text{
            color: #67c23a;
        }
    }
    .progress.is-exception{
        .progress__text{
            color: #f56c6c;
        }
    }
    .progress--circle{
        display: inline-block;
        position: relative;
        margin-right: 50px;
        .progress__text{
            position: absolute;
            top: 60%;
            left: 50%;
            transform: translate(-50%);
        }
    }
</style>
