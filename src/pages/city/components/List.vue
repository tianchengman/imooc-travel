<template>
  <div class="list" ref="wrapper">
    <div>
      <!-- better-scroll 包裹两层 -->
      <div class="area">
        <div class="title border-topbottom">当前城市</div>
        <div class="button-list">
          <div class="button-wrapper">
            <!-- <div class="button">{{this.$store.state.city}}</div> -->
            <div class="button">{{this.currentCity}}</div>
          </div>
        </div>
      </div>
      <div class="area">
        <div class="title border-topbottom">热门城市</div>
        <div class="button-list">
          <div
            class="button-wrapper"
            v-for="item of hot"
            :key="item.id"
            @click="handleCityClick(item.name)"
          >
            <div class="button">{{item.name}}</div>
          </div>
        </div>
      </div>
      <!-- cities it's object, so use (item, key) -->
      <!-- (item, key)-> key: a,b,c... :ref='a,b,c...' -->
      <div
        class="area"
        v-for="(item, key) of cities"
        :key="key"
        :ref="key"
      >
        <div class="title border-topbottom">{{key}}</div>
        <div class="item-list">
          <div
            class="item border-bottom"
            v-for="innerItem of item"
            :key="innerItem.id"
            @click="handleCityClick(innerItem.name)"
          >
            {{innerItem.name}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Bscroll from 'better-scroll'
import { mapState, mapMutations } from 'vuex'
export default {
	name: 'CityList',
	props: {
		hot: Array,
		cities: Object,
		letter: String
	},
	computed: {
    /* 将 vuex 共用数据 city 映射到 'currentCity' 当中 */
		...mapState({
			currentCity: 'city'
		})
	},
	mounted() {
    /* better-scroll:  */
		this.scroll = new Bscroll(this.$refs.wrapper)
	},
	methods: {
		handleCityClick(city) {
      /* 派发 actions: changeCity */
      // this.$store.dispatch('changeCity', city)
      /* 直接调用 mutations, 不需要通过 actions */
      // this.$store.commit('changeCity', city)
      this.changeCity(city)
      /* 页面跳转 */
			this.$router.push('/')
    },
    /* 将 vuex 共用 mutation 函数 changeCity 映射到 'changeCity' 当中: changeCity: changeCity */
		...mapMutations(['changeCity'])
	},
	watch: {
		letter() {
			/* if letter !== null */
			if (this.letter) {
				/* this.$refs[this.letter]: isArray */
				// console.log(this.$refs[this.letter])
				const element = this.$refs[this.letter][0]
				/* better-scroll: scrollToElement */
				this.scroll.scrollToElement(element)
			}
		}
	}
}
</script>

<style lang="stylus" scoped>
@import '~styles/varibles.styl';
@import './List.styl';
</style>
