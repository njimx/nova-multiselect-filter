<template>
    <div class="pt-2 pb-3">
        <h3 class="px-2 text-xs uppercase font-bold tracking-wide">
            {{ filter.name }}
        </h3>

        <div class="mt-1 px-3">
            <select-multiple
                :dusk="filter.name + '-filter-select'"
                class="block w-full form-control-sm form-select border border-gray-300 cursor-pointer relative"
                :options="filter.options"
                :value="value"
                @change="handleChange"
            />
        </div>

    </div>
</template>

<script>
    import SelectMultiple from './SelectMultiple'

    export default {
        components: {
            SelectMultiple
        },

        props: {
            resourceName: {
                type: String,
                required: true,
            },
            filterKey: {
                type: String,
                required: true,
            },
            lens: String,
        },

        methods: {
            handleChange(value) {
                this.$store.commit(`${this.resourceName}/updateFilterState`, {
                    filterClass: this.filterKey,
                    value: value,
                })

                this.$emit('change')
            },
        },

        computed: {
            filter() {
                return this.$store.getters[`${this.resourceName}/getFilter`](this.filterKey)
            },

            value() {
                return this.filter.currentValue
            },
        },
    }
</script>

