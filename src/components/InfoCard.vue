<template>
    
    <div>
        <!-- lg(含)以上會顯示 -->
        <b-container class="mt-5 d-none d-md-block">
            <b-row>
                <b-col md="4" v-for="(item, index) in infoList" :key="index">
                    <b-card
                        :title = item.siteName
                        :img-src = item.imgSrc
                        :img-alt = item.siteName
                        img-top
                        tag="article"
                        style="max-width: 20rem;"
                        class="mb-2"
                    >
                        <b-card-text>
                            溫度:{{ item.temperature }}<br/>
                            風向:{{ item.WindDirection }}<br/>
                            風量:{{ item.windPower }}<br/>
                            {{ item.gust }}級風<br/>
                            能見度:{{ item.visibility }}<br/>
                            濕度:{{ item.moisture }}<br/>
                            氣壓:{{ item.atmosphericPressure }}<br/>
                            天氣:{{ item.weather }}<br/>
                            降雨機率:{{ item.rainfall1day }}<br/>
                            測量單位:{{ item.unit }}<br/>
                            更新時間:{{ item.dataCreationDate }}<br/>
                        </b-card-text>

                        <b-button href="#" variant="primary">Go somewhere</b-button>

                    </b-card>
                </b-col>
                
            </b-row>
        </b-container>

        <!-- md(含)以下會顯示 -->
        <b-container class="d-block d-md-none mt-5" fuild>
            <b-list-group>

                <!-- v-for -->

                <b-list-group-item class="flex-column align-items-start"  v-for="(item, index) in infoList" :key="index">
                    <div class="d-flex w-100 justify-content-between">
                    <h5 class="mb-1">{{ item.siteName }}</h5>
                    <small>更新時間:{{ item.dataCreationDate }}</small>
                    </div>

                    <p class="mb-1">
                        溫度:{{ item.temperature }}<br/>
                        天氣:{{ item.weather }}<br/>
                        降雨機率:{{ item.rainfall1day }}<br/>

                    </p>

                    <small>測量單位:{{ item.unit }}</small>
                </b-list-group-item>


                

                
            </b-list-group>
        </b-container>
        
    </div>
    
</template>

<script>
export default {
    props:["infoList","locateImg"],
}
</script>

<style>

</style>