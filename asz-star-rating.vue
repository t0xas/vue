<template>
    <div @mouseout="mouseOut()">
        <div title="Сбросить" class="drop" @click="dropClick">&times;</div>
        <span v-for="(item,index) in items" :class="{ 'active': item.done }" :key="index" @mouseover="mouseOver(index)"
              @click="mouseClick(index)"></span>
    </div>
</template>
<script>
    export default {
        name: "asz-star-rating",
        props: ['count', 'input'],
        data: function () {
            let elements = [];
            for (let i = 0; i < this.count; i++) {
                elements.push({
                    done: false,
                    click: false
                });
            }
            return {
                items: elements,
            }
        },
        methods: {
            mouseOver: function (data) {
                for (let i = 0; i < this.count; i++) {
                    if (i <= data) {
                        this.items[i].done = true;
                    } else {
                        this.items[i].done = false;
                    }
                }
            },
            mouseOut: function () {
                this.items.forEach(function (item, index) {
                    if (!item.click) {
                        item.done = false;
                    }
                })
            },
            mouseClick: function (data) {
                document.querySelector(this.input).value=parseInt(data)+1;
                for (let i = 0; i < this.count; i++) {
                    if (i <= data) {
                        this.items[i].click = true;
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
        font-size: 15px;
        padding: 0;
        box-sizing: border-box;
    }
    div.drop:hover {
        background: red;
    }
</style>