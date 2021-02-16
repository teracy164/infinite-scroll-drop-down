<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <InfiniteScrollDropDown
          :items="items"
          :loadCompleted="data.length === loadedItemNum"
          :loading="loading"
          @next="onnext()" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import InfiniteScrollDropDown from './parts/InfiniteScrollDropDown'

  export default {
    name: 'HelloWorld',

    components: {
      InfiniteScrollDropDown,
    },

    mounted() {
      this.onnext()
    },

    data: () => {
      return {
        // MAX５０個のリスト
        data: new Array(50).fill().map((v, i) => ({value: i + 1, label: `item${i + 1}`})),
        items: [],
        loadedItemNum: 0,
        loading: false,
      }
    },

    methods: {
      onnext() {
        this.loading = true
        // 非同期での動作確認のため少し待ってから追加する
        setTimeout(() => {
          const step = 10;
          this.items.push(...this.data.slice(this.loadedItemNum, this.loadedItemNum + step));
          this.loadedItemNum += step;
          this.loading = false;
        }, 500);
      }
    }
  }
</script>
