<template>
  <div class="icons">
    <swiper :options="swiperOption">
      <swiper-slide v-for="(page, index) of pages" :key="index">
        <div
          class="icon"
          v-for="item of page"
          :key="item.id"
        >
          <div class='icon-img'>
            <img class='icon-img-content' :src='item.imgUrl'/>
          </div>
          <p class="icon-desc">{{item.desc}}</p>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</template>

<script>
export default {
	name: 'HomeIcons',
	props: {
		list: Array
	},
	data() {
		return {
			swiperOption: {
				autoplay: false
			}
		}
	},
	computed: {
    /**
     * @name: swiper 分页解决
     * @msg:
     * @param {type}
     * @return:
     */
		pages() {
      const pages = []
      // 分页
			this.list.forEach((item, index) => {
        // 每一样拥有的图标个数
				const page = Math.floor(index / 8)
				if (!pages[page]) {
					pages[page] = []
				}
				pages[page].push(item)
			})
			return pages
		}
	}
}
</script>

<style lang="stylus" scoped>
@import '~styles/varibles.styl';
@import './Icons.styl';
</style>
