<template>
  <div>
    <div class="search">
      <!-- 双向绑定: v-model -->
      <input
        class="search-input"
        type="text"
        v-model="keyword"
        placeholder="输入城市名或拼音"
      />
    </div>
    <div
      class="search-content"
      v-show="keyword"
      ref="search"
    >
      <ul>
        <li
          class="search-item border-bottom"
          v-for="item of list"
          :key="item.id"
          @click="handleCityClick(item.name)"
        >
          {{item.name}}
        </li>
        <li
          class="search-item border-bottom"
          v-show="hasNoData"
        >
          没有找到匹配数据
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Bscroll from 'better-scroll'
import { mapMutations } from 'vuex'
export default {
	name: 'CitySearch',
	props: {
		cities: Object
	},
	data() {
		return {
			keyword: '',
			list: [],
			timer: null
		}
	},
	computed: {
		hasNoData() {
			return !this.list.length
		}
	},
	mounted() {
		this.scroll = new Bscroll(this.$refs.search)
	},
	methods: {
		handleCityClick(city) {
			this.changeCity(city)
			this.$router.push('/')
		},
		...mapMutations(['changeCity'])
	},
	watch: {
		keyword() {
			if (this.timer) {
				clearTimeout(this.timer)
			}
			if (!this.keyword) {
				this.list = []
				return
			}
			/* 节流函数 */
			this.timer = setTimeout(() => {
				const result = []
				for (let i in this.cities) {
					this.cities[i].forEach(value => {
						/* can through spell and name search keyword */
						if (
							value.spell.indexOf(this.keyword) > -1 ||
							value.name.indexOf(this.keyword) > -1
						) {
							result.push(value)
						}
					})
				}
				this.list = result
			}, 100)
		}
	}
}
</script>

<style lang="stylus" scoped>
@import '~styles/varibles.styl';
@import './Search.styl';
</style>
