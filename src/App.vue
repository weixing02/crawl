<template>
  <div id="app">
    <el-button type="text" @click="dialogVisible = true">
      查看说明
    </el-button>
    <el-radio v-model="type" label="lazada">lazada</el-radio>
    <el-radio v-model="type" label="shopee">shopee</el-radio>
    <el-input
      class="input"
      placeholder="请输入数据"
      v-model="data"
      clearable
    ></el-input>
    <el-button class="button" type="primary" @click="generate">
      提取数据
    </el-button>
    <ul>
      <li v-for="(item, index) in urls" :key="index + item">
        {{ item }}
      </li>
    </ul>

    <el-dialog
      title="使用说明"
      :visible.sync="dialogVisible"
      width="50%"
      style="text-align: left;"
    >
      <div>
        需要手动复制数据，并黏贴到输入框中，点击【提取数据】按钮后生成链接
        操作步骤：
        <p>1. 进入对应的商品列表页面</p>
        <p>2. 打开调试器：【点击F12】或者【点击右键】-【检查】</p>
        <p>3. 选择Network（网络）</p>
        <p>4. 找到对应的请求接口（获取数据），并复制他们</p>
        <p>5. lazada和shopee对应的数据接口不同，可以参照：</p>
        <p>
          - lazada接口：https://www.lazada.co.th/[分类名称]
        </p>
        <p>- shopee接口： https://shopee.co.th/api/v4/recommend/recommend</p>
        <img src="./assets/intro1.png" style="width: 100%;" />
        <p>6. 黏贴到输入框中，点击【提取数据】即可</p>
        <img src="./assets/intro2.png" style="width: 100%;" />
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">我知道了</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      data: '',
      type: 'lazada', // 1 lazada  2 shopy
      urls: [],
      dialogVisible: false,
    }
  },
  components: {},
  methods: {
    generate() {
      if (this.type === 'lazada') {
        try {
          this.urls = JSON.parse(this.data).mods?.listItems?.map((item) => {
            return `https://${item.itemUrl}?${item.querystring}`
          })
        } catch {
          this.urls = ['数据错误']
        }
      } else {
        try {
          console.log(JSON.parse(this.data).data?.sections?.[0])
          this.urls = JSON.parse(
            this.data,
          ).data?.sections?.[0]?.data?.item?.map((item) => {
            const name = item.name.trim().replace(/\s+/g, '-')
            return `https://shopee.co.th/${name}-i.${item.shopid}.${item.itemid}`
          })
        } catch {
          this.urls = ['数据错误']
        }
      }
    },
    removeSetting(item) {
      var index = this.form.settings.indexOf(item)
      if (index !== -1) {
        this.form.settings.splice(index, 1)
      }
    },
    addSetting() {
      this.form.settings.push({
        name: '',
        key: '',
        val: '',
      })
    },
  },
  mounted() {},
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.input {
  width: 30vw;
  margin-right: 10px;
}

ul {
}

li {
  text-align: left;
  list-style: none;
  margin-top: 5px;
}
</style>
