# 全选反选

```vue
<template>
  <div>
    <span>全选:</span>
    <input type="checkbox" v-model="isAll" />
    <button @click="reverseBtn">反选</button>
    <ul>
      <li v-for="(obj,index) in arr" :key="index">
        <input type="checkbox" v-model="obj.c" />
        <span>{{obj.name}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false,
        },
        {
          name: "孙悟空",
          c: false,
        },
        {
          name: "唐僧",
          c: false,
        },
        {
          name: "白龙马",
          c: false,
        },
      ],
    };
  },
  computed: {
    'isAll': {
      set(value) {
        // value表示当前isAll的计算结果
        this.arr.forEach(obj => obj.c = value)
      },
      get() {
        return this.arr.every(obj => obj.c)
      }
    }
  },
  methods: {
    reverseBtn() {
      this.arr.forEach(obj => obj.c = !obj.c)
    }
  }

};
</script>
```

