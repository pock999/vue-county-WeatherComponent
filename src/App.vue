<template>
  <div id="app">
    <!-- 
      class=" d-lg-none"
      表示螢幕是lg的情形下，display是nane
     -->
    <b-alert show dismissible class="m-3 mb-5" >Default Alert</b-alert>
    <b-navbar toggleable="md" type="dark" variant="info">
        <b-navbar-brand href="#">
          <p class="h3 mt-2">
            <b-icon-brightness-alt-low></b-icon-brightness-alt-low>
            Weather
          </p>
        </b-navbar-brand>

        <b-navbar-toggle target="nav-collapse" class="ml-auto"></b-navbar-toggle>
        <!-- Navbar dropdowns -->
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ml-auto" >

            <b-nav-form class="mx-auto">
              <b-form-input size="sm" class="mr-sm-2" placeholder="搜尋縣市名"></b-form-input>
              <b-button size="sm" class="my-2 my-sm-0" type="submit">查詢</b-button>
            </b-nav-form>

            <b-nav-item-dropdown text="縣市" right>
              <!-- v-for -->
              <b-dropdown-item href="#">項目</b-dropdown-item>
              
            </b-nav-item-dropdown>

          </b-navbar-nav>
        </b-collapse>
    </b-navbar>

    <!-- 選用卡片，要放置自組件 -->
    <b-container>
      <b-row>
        <b-col lg="4" >
          <b-card
            title="Card Title"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
          >
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>

            <b-button href="#" variant="primary">Go somewhere</b-button>
          </b-card>
        </b-col>
        <b-col lg="4" >
          <b-card
            title="Card Title"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
          >
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>

            <b-button href="#" variant="primary">Go somewhere</b-button>
          </b-card>
        </b-col>
        <b-col lg="4" >
          <b-card
            title="Card Title"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
          >
            <b-card-text>
              Some quick example text to build on the card title and make up the bulk of the card's content.
            </b-card-text>

            <b-button href="#" variant="primary">Go somewhere</b-button>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
    
  </div>
</template>

<script>
// import HelloWorld from './components/InfoCard.vue'
export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data () {
    return {
      focusList:[],
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
