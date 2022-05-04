<template>
    <div tabindex="-1" class="flex h-auto">
        <div v-if="selected.length === 0" class="h-8 pt-1 leading-normal">&mdash;</div>
        <ul v-else ref="selected" class="list-reset flex flex-wrap text-sm -ml-2 pb-1 cursor-pointer">
            <li v-for="option, index in selected" :key="index" @click="remove(index)" :class="option.value" class="bg-gray-100 rounded -ml-0 mt-1 mr-1 px-2 py-1 hover:bg-gray-200">
                {{ option.label }}
            </li>
        </ul>

        <ul v-if="showDropdown && availableOptions.length" class="list-reset absolute w-5/6 py-1 border rounded-lg cursor-pointer bg-white z-10">
            <li v-for="option, index in availableOptions" :key="index" @click="select(option)" class="px-3 py-1 hover:bg-gray-100">
                {{ option.label }}
            </li>
        </ul>

    </div>
</template>

<script>
    export default {
        props: {
            options: {
                type: Array,
                default: []
            },
            value: {
                default: ''
            }
        },
        data: () => ({
            showDropdown: false,
            selected: []
        }),
        computed: {
            availableOptions() {
                return this.options.filter(option => !this.selected.includes(option));
            }
        },
        methods: {
            select(option) {
                this.showDropdown = false;
                this.selected.push(option);
            },
            remove(index) {
                this.selected.splice(index, 1);
            },
            toggle(event) {
                if([this.$el, this.$refs.selected].includes(event.target) && this.availableOptions.length > 0) {
                    this.showDropdown = !this.showDropdown;
                }
                else {
                    this.showDropdown = false;
                }
            }
        },
        mounted() {
            document.addEventListener('click', this.toggle);
            this.selected = this.options.filter(option => {
                return this.value.includes(option.value);
            });
        },
        watch: {
            selected:{
                handler:  function() {
                    this.$emit('change', this.selected.map(e => e.value));
                },
                deep: true
            }
        }
    }
</script>
