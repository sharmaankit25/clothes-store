<template>
    <div>
        <ul class="list filter-list">
            <li>
                <strong>Filter</strong>
                <div v-if="Object.keys(selectedFilters).length">
                    <span>Applied Filters</span>
                    <ul class="list">
                        <li v-for="filter in Object.keys(selectedFilters)" :key="filter">
                            <span>{{ filter }} : </span>
                            <span>{{ selectedFilters[filter].join(',') }}</span>
                        </li>
                    </ul>
                    <button @click="selectedFilters = {}">Clear All</button>
                </div>
            </li>
            <li v-for="filter in filters" :key="filter.filter_lable">
                <span>{{ filter.filter_lable }}</span>
                <ul class="list filter-sublist " v-if="filter.options">
                    <li
                    :class="selectedFilters[option.code] && selectedFilters[option.code].includes(option.value_key) && 'selected-filter-list-item'"
                    @click="selectFilter(option.code, option.value_key)"
                    class="sub-list-item" v-for="option in filter.options"
                    :key="option.value_key">
                        <span class="color" :style="`background-color: ${option.value_key}`">
                        </span>
                        <span class="color-text">{{ option.value }}</span>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data () {
        return {
            selectedFilters: {}
        }
    },
    methods: {
        selectFilter (filter, value) {
            if (this.selectedFilters[filter]) {
                if (this.selectedFilters[filter].includes(value)) {
                    this.selectedFilters[filter] = this.selectedFilters[filter].filter(item => item !== value)
                } else {
                    const newValues = this.selectedFilters[filter]
                    this.selectedFilters = { ...this.selectedFilters, [filter]: [...new Set([...newValues, value])] }
                }
            } else {
                this.selectedFilters = { ...this.selectedFilters, [filter]: [value] }
            }
        }
    },
    watch: {
        selectedFilters () {
            this.$emit('select', this.selectedFilters)
        }
    },
    props: {
        filters: Array
    }
}
</script>

<style scoped>
    .filter-list {
        width: 220px;
    }

    .selected-filter-list-item {
        background-color: gray;
        color: #fff;
    }

    .filter-sublist {
        max-height: 300px;
        overflow-y: scroll;
        overflow: auto;
    }

    .color {
        display:inline-block;width:50px;height:25px;
        text-align: left;
        margin: 2px;
    }

    .color-text {
        text-align: left;
        margin: 2px;
    }

    .sub-list-item {
        text-align: left;
        cursor: pointer;
    }
</style>
