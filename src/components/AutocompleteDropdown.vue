<template>
    <div class="dropdown" :class="{'open' : open}">
        <input type="text" v-model="searchText" @input="searchChanged" @keydown.down="down" @keydown.up="up" @keydown.enter="suggestionSelected(matches[highlightIndex])"/>
        <a class="toggle" @mousedown.prevent @click="setOpen(!open)">
            <span class="arrow-up">▲</span>
            <span class="arrow-down">▼</span>
        </a>
        <ul class="suggestion-list">
            <li v-for="(suggestion, index) in matches" :key='index' @mousedown.prevent @click="suggestionSelected(suggestion)">
                {{ suggestion[0] }}
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        props: ['options'],
        data() {
            return {
                searchText: '',
                selectedOption: null,
                open: false,
                highlightIndex: 0
            }
        },
        methods: {
            setOpen(isOpen) {
                this.open = isOpen
            },
            suggestionSelected(suggestion) {
                this.open = false
                this.searchText = suggestion[0]
                this.$emit('input', suggestion[1])
            },
            updateComponentWithValue(newValue) {
                if (Object.values(this.options).indexOf(newValue) > -1) {
                    // Find the matching text for the supplied option value
                    for (var text in this.options) {
                        if (this.options.hasOwnProperty(text)) {
                            if (this.options[text] === newValue) {
                                this.searchText = text
                            }
                        }
                    }
                }
            },
            searchChanged() {
                if (!this.open) {
                    this.open = true
                }
    
                this.highlightIndex = 0
            },
            up() {
                if (this.open) {
                    if (this.highlightIndex > 0) {
                        this.highlightIndex--
                    }
                } else {
                    this.setOpen(true)
                }
            },
    
            down() {
                if (this.open) {
                    if (this.highlightIndex < this.matches.length - 1) {
                        this.highlightIndex++
                    }
                } else {
                    this.setOpen(true)
                }
            },
        },
        computed: {
            matches() {
                return Object.entries(this.options).filter((option) => {
                    var optionText = option[0].toUpperCase()
                    return optionText.match(this.searchText.toUpperCase())
                })
            }
        },
        mounted() {
            this.updateComponentWithValue(this.value)
        },
        watch: {
            value: function(newValue) {
                this.updateComponentWithValue(newValue)
            }
        },
    }
</script>

<style scoped>
    .dropdown {
        display: inline-block;
        position: relative;
    }
    
    .suggestion-list {
        background-color: rgba(255, 255, 255, 0.95);
        border: 1px solid #ddd;
        list-style: none;
        display: block;
        margin: 0;
        padding: 0;
        width: 100%;
        overflow: hidden;
        position: absolute;
        top: 20px;
        left: 0;
        z-index: 2;
    }
    
    .dropdown.open .suggestion-list {
        display: block;
    }
    
    .dropdown .suggestion-list {
        display: none;
    }
    
    .toggle .arrow-up {
        display: none;
    }
    
    .open .toggle .arrow-up {
        display: inline-block;
    }
    
    .open .toggle .arrow-down {
        display: none;
    }
    
    .suggestion-list li {
        cursor: pointer;
    }
    
    .suggestion-list li:hover {
        color: #fff;
        background-color: rgb(103, 97, 143);
    }
    
    .active {
        color: #fff;
        background-color: #42b983;
    }
</style>
