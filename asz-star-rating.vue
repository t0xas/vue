<template>
    <div @mouseout="mouseOut()">
        <div v-show="input" title="Сбросить" class="drop" @click="dropClick">&times;</div>
        <span :style="{ cursor: cursorStyle }"  v-for="(item,index) in items" :class="{ 'active': item.done }" :key="index" @mouseover="mouseOver(index)"
              @click="mouseClick(index)"></span>
    </div>
</template>
<script>
    export default {
        name: "asz-star-raiting",
        props: ['count', 'input', 'static', 'value'],
        data: function () {
            let cs = 'pointer';
            this.items = [];
            for (let i = 0; i < this.count; i++) {
                this.items.push({
                    done: false,
                    click: false
                });
            }
            if(this.static == 'true') {
                this.markRating(this.value);
                cs = 'default';
            }
            return {
                cursorStyle: cs
            }
        },
        methods: {
            markRating: function (count) {
                for (let i = 0; i < this.count; i++) {
                    if (i < count) {
                        this.items[i].done = true;
                    } else {
                        this.items[i].done = false;
                    }
                }
            },
            mouseOver: function (data) {
                if(this.static == undefined) {
                    for (let i = 0; i < this.count; i++) {
                        if (i <= data) {
                            this.items[i].done = true;
                        } else {
                            this.items[i].done = false;
                        }
                    }
                }
            },
            mouseOut: function () {
                if(this.static == undefined) {
                    this.items.forEach(function (item, index) {
                        if (!item.click) {
                            item.done = false;
                        }
                    })
                }
            },
            mouseClick: function (data) {
                if(this.static == undefined) {
                    document.querySelector(this.input).value = parseInt(data) + 1;
                    for (let i = 0; i < this.count; i++) {
                        if (i <= data) {
                            this.items[i].click = true;
                        }
                    }
                }
            },
            dropClick: function () {
                document.querySelector(this.input).value='';
                for (let i = 0; i < this.count; i++) {
                    this.items[i].click = false;
                    this.items[i].done = false;
                }
            }
        }
    }
</script>
<style scoped>
    span {
        content: ' ';
        background: url('lapki.png') no-repeat -22px 0;
        background-size: cover;
        display: inline-block;
        width: 20px;
        height: 17px;
        cursor: pointer;
    }
    span.active {
        background-position: 0 0;
    }
    div.drop {
        display: inline-block;
        color: red;
        cursor: pointer;
        position: relative;
        top:-2px;
        border: 1px solid silver;
        border-radius: 50%;
        line-height: 100%;
        text-align:center;
        width: 17px;
        height: 17px;
        font-weight: bold;
        font-size: 14px;
        padding: 0;
        box-sizing: border-box;
    }
    div.drop:hover {
        background: red;
        color: #fff;
    }
</style>