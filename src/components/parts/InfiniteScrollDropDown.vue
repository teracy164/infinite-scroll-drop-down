<template>
    <v-select
        :items="items"
        :label="label"
        item-text="label"
        item-value="value"
        :menu-props="{'content-class': listClass}"
        @click="onclick">
        <template v-if="!loadCompleted" v-slot:append-item>
            <v-divider class="mb-2"></v-divider>
            <v-list-item v-if="!loading" @click="onclickload">
                load
            </v-list-item>
            <v-list-item v-else>
                loading...
            </v-list-item>
        </template>
    </v-select>
</template>

<script>
  export default {
    props: {
        items: {
            type: Array,
            default: () => [],
        },
        label: {
            type: String,
            default: '',
        },
        loadCompleted: {
            type: Boolean,
            default: false,
        },
        loading: {
            type: Boolean,
            default: false,
        },
    },

    data() {
        return {
            listClass: 'my-select-list',
        };
    },

    methods: {
        onclick() {
            let cnt = 0;
            const getList = () => {
                if (++cnt > 20) return;

                const elements = document.getElementsByClassName(this.listClass);
                if (elements.length) {
                    const menu = elements[0];
                    menu.addEventListener('scroll', () => {
                        if (menu.scrollHeight - (menu.clientHeight + menu.scrollTop) < 5) {
                            this.scrollend()
                        }
                    });
                } else {
                    setTimeout(() => getList(), 100);
                }
            };

            getList();
        },
        scrollend() {
            if (!this.loadCompleted) {
                this.$emit('next');
            }
        },
        onclickload() {
            this.$emit('next');
        }
    }
  }
</script>
