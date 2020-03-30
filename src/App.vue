<template>
  <div id="app">
    <!-- 
      class=" d-lg-none"
      表示螢幕是lg的情形下，display是nane
     -->
    <b-alert show dismissible class="m-3 mb-5" >
      資料來源:<a href="https://opendata.epa.gov.tw/Data/Details/ATM00698/?show=all">行政院環境保護署。環境資源資料開放平臺</a>
      <br/>
      <a href="https://data.gov.tw/license">政府資料開放授權條款</a>
    </b-alert>
    <b-navbar toggleable="md" type="dark" variant="info">
        <b-navbar-brand href="#">
          <p class="h3 mt-2" onclick="window.location.href=window.location.href">
            <b-icon-brightness-alt-low></b-icon-brightness-alt-low>
            Weather
          </p>
        </b-navbar-brand>

        <b-navbar-toggle target="nav-collapse" class="ml-auto"></b-navbar-toggle>
        <!-- Navbar dropdowns -->
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ml-auto" >

            <b-navbar-nav class="mx-auto">
              <input size="sm" class="mr-sm-2" 
              placeholder="搜尋縣市名" v-model="searchCounty"
              @keyup.enter="keywordSearch"
              :disabled="isDisable">
              <b-button size="sm" class="my-2 my-sm-0" type="button" 
              @click="keywordSearch" :disabled="isDisable">
                查詢
              </b-button>
            </b-navbar-nav>

            <b-nav-item-dropdown text="縣市" right>
              <!-- v-for -->
              <b-dropdown-item v-for="(item, index) in ajaxCountyList" :key="index"
              @click="keywordSearch(item)">
                {{ item }}
              </b-dropdown-item>
              
            </b-nav-item-dropdown>

          </b-navbar-nav>
        </b-collapse>
    </b-navbar>

    <div class="text-center mt-3" :class="{ hide: isHide }">
      <b-spinner variant="primary" label="Text Centered"></b-spinner>
    </div>

    <div class="justify-content-center mt-3" :class="{ HasResult: hasResult }">
      <b-spinner class="no-result-icon" label="Large Spinner" type="grow"></b-spinner>
      <b-icon-backspace-reverse-fill class="no-result-icon"></b-icon-backspace-reverse-fill>
      <span class="no-result-font">沒找到ㄟ</span>
      <b-spinner class="no-result-icon" label="Large Spinner" type="grow"></b-spinner>
    </div>

    <InfoCard
    :info-list="renderList"
    :locate-img="locationImg"
    :like-list="focusList"
    @updateLikeList="updateFocusList"
    ></InfoCard>
    
  </div>
</template>

<script>
// https://cors-anywhere.herokuapp.com/http://opendata.epa.gov.tw/webapi/Data/ATM00698/?$skip=0&$top=1000&format=json
import InfoCard from './components/InfoCard.vue'
export default {
  name: 'App',
  components: {
    InfoCard,
  },
  data () {
    return {
      /** 載入圖示顯示 **/
      isHide:true,

      /** 表示input的disable狀態 **/
      isDisable:true,

      /** 搜璇沒結果才顯示 **/
      hasResult:true,

      /** 一開始ajax獲得的的list **/
      initList:[],

      /** 代表要渲染的list **/
      renderList:[],

      /** 表示目前ajax抓下來的資料內所有的縣市 **/
      ajaxCountyList:[],

      /** 搜尋縣市名輸入框 **/
      searchCounty:"",

      /** 代表關注的list **/
      focusList:[],

      /** 代表各地fakeimg **/
      locationImg:[
        {location:"南沙島", img: "https://fakeimg.pl/200x100/38CAE8/EEEEEE/?retina=2&text=南沙島&font=noto"},
        {location:"東吉島", img: "https://fakeimg.pl/200x100/38E8A5/EEEEEE/?retina=3&text=東吉島&font=noto"},
        {location:"東沙島", img: "https://fakeimg.pl/200x100/38E877/EEEEEE/?retina=3&text=東沙島&font=noto"},
        {location:"馬祖", img: "https://fakeimg.pl/200x100/B623E8/EEEEEE/?retina=4&text=馬祖&font=noto"},
        {location:"金門", img: "https://fakeimg.pl/200x100/E89213/EEEEEE/?retina=5&text=金門&font=noto"},
        {location:"澎湖", img: "https://fakeimg.pl/200x100/109CE8/EEEEEE/?retina=6&text=澎湖&font=noto"},
        {location:"蘭嶼", img: "https://fakeimg.pl/200x100/1F75FF/EEEEEE/?retina=7&text=蘭嶼&font=noto"},
        {location:"大武", img: "https://fakeimg.pl/200x100/46CA41/EEEEEE/?retina=8&text=大武&font=noto"},
        {location:"臺東", img: "https://fakeimg.pl/200x100/22E80C/EEEEEE/?retina=9&text=臺東&font=noto"},
        {location:"成功", img: "https://fakeimg.pl/200x100/6F0DFF/EEEEEE/?retina=10&text=成功&font=noto"},
        {location:"花蓮", img: "https://fakeimg.pl/200x100/20E8AF/EEEEEE/?retina=11&text=花蓮&font=noto"},
        {location:"恆春", img: "https://fakeimg.pl/200x100/0D6FFF/EEEEEE/?retina=12&text=恆春&font=noto"},
        {location:"高雄", img: "https://fakeimg.pl/200x100/0DAFBE/EEEEEE/?retina=13&text=高雄&font=noto"},
        {location:"臺南", img: "https://fakeimg.pl/200x100/FFA60D/EEEEEE/?retina=14&text=臺南&font=noto"},
        {location:"嘉義", img: "https://fakeimg.pl/200x100/E88058/EEEEEE/?retina=15&text=嘉義&font=noto"},
        {location:"阿里山", img: "https://fakeimg.pl/200x100/BF8D7E/EEEEEE/?retina=16&text=阿里山&font=noto"},
        {location:"玉山", img: "https://fakeimg.pl/200x100/95BCBF/EEEEEE/?retina=17&text=玉山&font=noto"},
        {location:"日月潭", img: "https://fakeimg.pl/200x100/78BCFF/EEEEEE/?retina=18&text=日月潭&font=noto"},
        {location:"梧棲", img: "https://fakeimg.pl/200x100/90D9E8/EEEEEE/?retina=19&text=梧棲&font=noto"},
        {location:"臺中", img: "https://fakeimg.pl/200x100/7788BF/EEEEEE/?retina=20&text=臺中&font=noto"},
        {location:"新竹", img: "https://fakeimg.pl/200x100/EBB47F/EEEEEE/?retina=21&text=新竹&font=noto"},
        {location:"新屋", img: "https://fakeimg.pl/200x100/4E97BF/EEEEEE/?retina=22&text=新屋&font=noto"},
        {location:"板橋", img: "https://fakeimg.pl/200x100/A350BF/EEEEEE/?retina=23&text=板橋&font=noto"},
        {location:"臺北", img: "https://fakeimg.pl/200x100/F5564B/EEEEEE/?retina=24&text=臺北&font=noto"},
        {location:"陽明山", img: "https://fakeimg.pl/200x100/F5B4F1/EEEEEE/?retina=25&text=陽明山&font=noto"},
        {location:"鞍部", img: "https://fakeimg.pl/200x100/E0E0AF/EEEEEE/?retina=26&text=鞍部&font=noto"},
        {location:"蘇澳", img: "https://fakeimg.pl/200x100/7BD0DF/EEEEEE/?retina=27&text=蘇澳&font=noto"},
        {location:"宜蘭", img: "https://fakeimg.pl/200x100/7EB1DF/EEEEEE/?retina=28&text=宜蘭&font=noto"},
        {location:"基隆", img: "https://fakeimg.pl/200x100/FF6761/EEEEEE/?retina=29&text=基隆&font=noto"},
        {location:"彭佳嶼", img: "https://fakeimg.pl/200x100/5E78FF/EEEEEE/?retina=30&text=彭佳嶼&font=noto"},
        {location:"其他", img: "https://fakeimg.pl/200x100/dddddd/EEEEEE/?retina=30&font=noto"},
      ],
    }
  },
  methods:{
    /** 子組件emit方法，用來更新關注的list **/
    updateFocusList:function(newLikeList){
      this.focusList = newLikeList;
    },
    /** 根據地點名稱在locationImg找到相對應的圖片 **/
    findImage:function(locate){
      let resdata;
      try{
        resdata = JSON.parse(JSON.stringify(this.locationImg.find(ele => ele.location == locate)));
      }catch(e){
        resdata = JSON.parse(JSON.stringify(this.locationImg.find(ele => ele.location == "其他")));
      }
      return resdata.img;
    },
    keywordSearch:function(item){
      const searchList = [];
      if(item != ''){
        this.searchCounty = item;
      }
      this.initList.forEach(element => {
        if(element.siteName.includes(this.searchCounty)){
          searchList.push(element);
        }
      });
      if(searchList.length == 0){
        this.hasResult = false;
      }else{
        this.hasResult = true;
      }
      this.renderList = searchList;
    },
  },
  beforeMount: function() {
    this.isHide = false;
  },
  created: function() {
    console.log("init ajax start");
    const BreakException ={};
    this.axios.get('https://cors-anywhere.herokuapp.com/http://opendata.epa.gov.tw/webapi/Data/ATM00698/?$skip=0&$top=400&format=json')
    .then((res) => { 
      const currentdate = new Date(); 
      let creationDateFormat;
      if(currentdate.getMinutes() > 30){
        creationDateFormat = (currentdate.getFullYear()-1911) + '/' + (currentdate.getMonth()+1) + 
       '/' + currentdate.getDate() + ' ' + currentdate.getHours() + ':00:00';
      }else{
        creationDateFormat = (currentdate.getFullYear()-1911) + '/' + (currentdate.getMonth()+1) + 
       '/' + currentdate.getDate() + ' ' + (currentdate.getHours()-1) + ':00:00';
      }
      const newList = [];
      res.data.forEach((element) => {
        if(element.DataCreationDate == creationDateFormat){

          let isHave = false;
          try {
            newList.forEach(ele => { 
              if(ele.siteName == element.SiteName){
                isHave = true;
                throw BreakException;
              }
            });
          }catch (e) {
            if (e !== BreakException) throw e;
          }
          
          if(!isHave){
            this.ajaxCountyList.push(element.SiteName);
            newList.push({
              siteName : element.SiteName,
              windDirection: element.WindDirection,
              windPower: element.WindPower,
              gust: element.Gust,
              visibility: element.Visibility,
              temperature: element.Temperature,
              moisture: element.Moisture,
              atmosphericPressure: element.AtmosphericPressure,
              weather: element.Weather,
              rainfall1day: element.Rainfall1day,
              unit: element.Unit,
              dataCreationDate: element.DataCreationDate,
              imgSrc: this.findImage(element.SiteName),
            });
          }
          
        }
      });
      this.isHide = true;
      this.initList = newList;
      this.renderList = newList;
      this.isDisable = false;
      this.hasResult = true;

      console.log("create ajax end");
    })
    .catch((error) => { 
      console.error(error);
      this.hasResult = false;
      alert("sorry ajax 出問題");
     });
  }
}
  


</script>

<style>
*{
  font-family: '微軟正黑體', Courier, monospace;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.hide{
  display: none;
}
.no-result-icon{
  width: 3rem; 
  height: 3rem;
}
.no-result-font{
  font-size: 2.5rem; 
  line-height: 3rem;
}
.HasResult{
  display: none;
}
</style>
