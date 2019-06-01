<template>
  <div>
    <ul>
      <li @click="gotoDetail()" class="movie" v-for="book in booklist" :key="book.id">
        <div class="movie-img">
          <img :src="book.book_cover" alt>
        </div>
        <div class="movie-info">
          <div class="movie-info-title">{{book.bookname
}}</div>
          <div>状态:
            <span class="movie-info-average">{{book.stat_name}}</span>
          </div>
          <div class="movie-info-star">
            作者:
            <span>{{book.author_name}}</span>
          </div>
           <div>类型:
            <span>{{book.class_name}}</span>
          </div>
        </div>
      </li>
    </ul>
    <div class="end" v-show="isEnd">
      <h3>数据到底了</h3>
    </div>
    <div class="loading" v-show="isLoading">
      <img src="@/assets/img/loading.gif" alt>
    </div> 
  </div>
</template>


<script>
import axios from "axios";
export default {
  data() {
    return {
      booklist: [],
       isLoading: true,
      isEnd: false,
    };
  },
  methods: {
    getData() {
      let url = "https://bird.ioliu.cn/v2?url=https://www.apiopen.top/novelApi";
       this.isLoading = true;
      axios.get(url).then(res => {
        console.log(res);
         let getList = res.data.data.slice(
          this.booklist.length,
          this.booklist.length + 5,
        );
        if (getList.length < 5) {
          this.isEnd = true;
        }
        this.booklist = this.booklist.concat(getList);
        this.isLoading = false;
      });
    }
  },
  created() {
    this.$emit("switchTab", "book");
    this.getData();
  },
   mounted() {
    window.onscroll = () => {
      let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
      let scrollHeight = document.documentElement.scrollHeight;
      let clientHeight = document.documentElement.clientHeight;
      // console.log(scrollHeight, scrollTop, clientHeight);
      if (scrollHeight == scrollTop + clientHeight && !this.isEnd) {
        // 请求数据
        this.getData();
      }
    };
  }
};
</script>

<style lang="scss" scoped>
.movie {
  display: flex;
  padding: 0.2rem;
  border-bottom: 0.02rem solid #ccc;
  overflow: hidden;
  &-img {
    flex-grow: 1;
    width: 0;
    img {
      width: 100%;
    }
  }
  &-info {
    flex-grow: 3;
    width: 0;
    margin-left: 0.2rem;
    &-title {
      color: #333;
      font-weight: 700;
      font-size: 0.34rem;
    }
    &-average {
      font-weight: 700;
      color: #faaf00;
    }

    &-star {
      color: #666;
      font-size: 0.26rem;
    }
  }
   a{
      display: block;
      width: 100%;
      height: 100%;
    }
}

.loading {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 2.2rem;
  height: 2.2rem;
  img {
    width: 90%;
    position: absolute;
    padding: 0.1rem;
    text-align: center;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 0.2rem;
  }
}

.end {
  text-align: center;
}
</style>
