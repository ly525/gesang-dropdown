<template>
    <i-poptip trigger="click" placement="bottom" class="custom-poptip" :width="contentWidth">
        <i-button type="ghost" style="width:100%;">
            <span class="gesang-dropdown-title">{{title}}</span>
            <i-icon type="arrow-down-b"></i-icon>
        </i-button>
        <div slot="content" class="gesang-dropdown-content">
            <div class="gesang-dropdown-content-header" :style="{width:headerWidthPercent+'%'}">
                <div class="gesang-dropdown-search">
                    <input v-model="filtedValue" class="gesang-dropdown-input" placeholder="Search..." autofocus/>
                </div>
                <div class="gesang-dropdown-shortcut">
                    <i-button size="small" @click="handleSelectAll(true)">Select All</i-button>
                    <i-button size="small" @click="handleSelectAll(false)">Diselect All</i-button>
                </div>
            </div>
            <div class="gesang-dropdown-content-list">
                <ul>
                    <li v-for="(key, index) in filtedKeyList" :key="index" style="cursor: pointer;"
                        @click="updatecheckedList(key)">
                        <div class="gesang-dropdown-item-wrapper">
                            <div class="gesang-dropdown-item-name" :style="{flex: nameRatio}" 
                                 v-html="isMap ? fullKeyValueMap[key].replace(filtedValue, markToken): ('' + key).replace(filtedValue, markToken)"
                                 :title="isMap ? fullKeyValueMap[key]: key">
                            </div>
                            <div class="gesang-dropdown-item-checkbox" :style="{flex: checkboxRatio}">
                                <input type="checkbox" :value="key" v-model="checkedList">
                                <label :for="key"></label>
                            </div>
                        </div>
                    </li>
                </ul>
            </div>

        </div>
    </i-poptip>
</template>
<script>
    import iPoptip from 'iview/src/components/poptip'
    import iButton from 'iview/src/components/button'
    import iIcon from 'iview/src/components/icon'
    // import iInput from 'iview/src/components/input'
    export default {
        name: 'GesangDropdown',
        props: {
            prop: {
                default: '',
            },
            type: {
                default: 'map',
            },
            titlePrefix: {
                default: 'Has selected',
            },
            titleSuffix: {
                default: '',
            },
            initCheckedList: {
                type: Array,
                default() {
                    return []
                },
            },
            fullList: {
                type: Array,
                default() {
                    return []
                },
            },
            fullKeyValueMap: {
                type: Object,
                default() {
                    return {}
                },
            },
            onChecked: {
                type: Function,
                default() {
                    return {}
                },
            },
            contentWidth: {
                type: Number,
                default: 200,
            },
            nameRatio: {
                type: [Number, String],
                default: 2,
            },
            checkboxRatio: {
                type: [Number, String],
                default: 1,
            },
            headerWidthPercent: {
                default: 75
            }
        },
        data() {
            return {
                filtedValue: '',
                checkedList: this.initCheckedList.slice(0),
            }
        },
        computed: {
            title() {
                return `${this.titlePrefix} ${this.checkedList.length} ${this.titleSuffix || this.prop}`
            },
            placeholder() {
                return `Select ${this.prop}`
            },
            filtedKeyList() {
                return !this.filtedValue ? this.fullList : this.fullList.filter(item => {
                    if (this.isMap) {
                        return this.fullKeyValueMap['' + item].includes(this.filtedValue)
                    } else {
                        return ('' + item).includes(this.filtedValue)
                    }
                })
            },
            isMap() {
                return this.type === 'map'
            },
            fullValueKeyMap() {
                return Object.entries(this.fullKeyValueMap).reduce((result, [key, value]) => {
                    result[value] = key
                    return result
                }, {})
            },
        },
        methods: {
            markToken(match, variable) {
                return `<span style="color: orange;">${match}</span>`
            },
            updatecheckedList(key) {
                if (this.checkedList.includes(key)) {
                    const index = this.checkedList.indexOf(key)
                    this.checkedList.splice(index, 1)
                } else {
                    this.checkedList.push(key)
                }
                this.onChecked(this.checkedList, this.prop)
            },
            handleSelectAll(isAll) {
                // notice: When select all, we need to slice the filtedList for backup the list.
                // if not, when you update checkedList(remove item from the checked list, the filted list will also remove the item => boom bug)
                this.checkedList = isAll ? this.filtedKeyList.slice(0) : []
                this.onChecked(this.checkedList, this.prop)
            },
        },
        components: {
            iPoptip,
            iButton,
            iIcon,
            // iInput,
        },
    }
</script>
<style lang="scss">
    .gesang-dropdown-title {
        padding-right: 5px;

    }

    div.gesang-dropdown-content {
        position: relative;
        overflow-y: scroll;

        .gesang-dropdown-input {
            display: inline-block;
            width: 100%;
            height: 32px;
            line-height: 1.5;
            padding: 4px 7px;
            font-size: 12px;
            border: 1px solid #dddee1;
            border-radius: 4px;
            color: #495060;
            background-color: #fff;
            background-image: none;
            position: relative;
            cursor: text;
            transition: border .2s ease-in-out, background .2s ease-in-out, box-shadow .2s ease-in-out;
            &:focus {
                outline: none;
            }
        }
        .gesang-dropdown-content-header {
            position: absolute;
            z-index: 9;

            width: 75%;

            .gesang-dropdown-search {
                margin-bottom: 5px;

            }

            .gesang-dropdown-shortcut {
                button {
                    width: 50%;
                }
            }
        }
        .gesang-dropdown-content-list {
            margin-top: 60px;
            height: 20em;
            overflow-y: scroll;
            overflow-x: hidden;
        }
        /*overflow-x: hidden;*/
        .gesang-dropdown-item-wrapper {
            /*position: relative;*/
            display: flex;
            flex-direction: row;
            padding: 8px;
            /*width: 50%;*/

            .gesang-dropdown-item-name {
                flex: 2;
                /*max-width: 40%;*/
                /*width: 50%;*/
                text-align: left;
                text-overflow: ellipsis;
                white-space: nowrap;
                overflow: hidden;
            }
            .gesang-dropdown-item-checkbox {
                flex: 1;

                position: relative;
                /*top: 0;*/
                /*right: 0;*/

                input[type=checkbox] {
                    visibility: hidden;
                }

                label {
                    position: absolute;
                    top: 0;
                    left: 0;
                    width: 25px;
                    height: 25px;
                    background: white;
                    cursor: pointer;
                    &:after {
                        position: absolute;
                        top: 10px;
                        right: 7px;
                        width: 9px;
                        height: 5px;
                        opacity: 1;
                        content: '';
                        border: 2px solid #bdbdbd;
                        border-top: none;
                        border-right: none;
                        background: transparent;
                        -webkit-transform: rotate(-45deg);
                        -moz-transform: rotate(-45deg);
                        -o-transform: rotate(-45deg);
                        -ms-transform: rotate(-45deg);
                        transform: rotate(-45deg);
                    }

                }
                input[type=checkbox]:checked + label:after {
                    opacity: 1;
                    border: 2px solid #2db7f5;
                    border-top: none;
                    border-right: none;

                }
            }
        }
    }

    div.ivu-poptip-body {
        padding-left: 20px;
        padding-right: 0;
    }

    .custom-poptip {
        width: 100%;
        div.ivu-poptip-rel {
            width: 100%;
        }
    }

</style>
