<template>
    <div class="col-lg-9 px-0" id="news">
        <div class="row">
            <h4 class="font-weight-bold mx-3 mt-4"><i class="fa fa-newspaper-o"></i> Lastest Sport's News</h4>
        </div>
        <div class="row">
            <div class="col-lg-6 px-0 lastest-news" v-for="nw in news" v-show="nw.description != '' && nw.title != ''">
                <a :href="nw.url" target="_blank" class="card ml-3 my-2">
                    <img class="card-img-top" :src="nw.urlToImage">
                    <div class="card-body p-2">
                        <p class="card-title" v-html="nw.title"></p>
                        <p class="card-text" v-html="getShortDescription(nw.description)"></p>
                        <p class="font-weight-bold float-left">Source: {{ nw.source.name }}</p>
                        <p class="font-italic float-right">{{ getPostTime(nw.publishedAt) }}</p>
                    </div>
                </a>
            </div>
        </div>      
    </div>


</template>

<script>
    var request = new XMLHttpRequest();
    var contents = [];
    // var loadContents = [];
    var contentCount = 0;
    var pageCount = 2;
    var fApiKey = "955acf3993df49169dfa33dce76d015f";
    var sApiKey = "295e708b17e644599a2794b5fae59e5f"; 
    // var url = "http://worldcup.sfg.io/matches";
    // var url = "https://newsapi.org/v2/everything?sources=football-italia&apiKey=" + fApiKey; 
    // var url = "https://newsapi.org/v2/everything?sources=bbc-sport&apiKey=" + fApiKey;
    // var url = "https://newsapi.org/v2/everything?sources=fox-sports&apiKey=" + fApiKey;
    // var url = "https://newsapi.org/v2/everything?sources=talksport&apiKey=" + fApiKey;
    // var url = "https://newsapi.org/v2/top-headlines?sources=the-sport-bible&apiKey=" + fApiKey;
    // var url = "https://newsapi.org/v2/top-headlines?sources=four-four-two&apiKey=" + fApiKey;
    var url = "https://newsapi.org/v2/everything?sources=bleacher-report&apiKey=" + fApiKey;

    request.onreadystatechange = function() {
        if(request.readyState === 4) {
            if(request.status === 200) {
                let response = JSON.parse(request.responseText); 
                let status = response.status;
                if(status = "ok") {
                    for(let i = 0; i < 20; i++) {
                        contents.push(response.articles[i]);  
                    }
                }
            }
            else {
                console.log('An error occurred during your request: ' +  request.status + ' ' + request.statusText);
            } 
        }
    }
    request.open('GET', url);
    request.send();
    
    window.onscroll = function(ev) {
        if ((window.innerHeight + window.scrollY) >= document.body.offsetHeight) {
            // alert("you're at the bottom of the page");
            if(pageCount > 2) { 
                let tmp = pageCount - 1;
                let previousPage = "&page=" + tmp.toString();
                let nextPage = "&page=" + pageCount.toString();
                url = url.replace(previousPage, nextPage);
            }
            else url = url + "&page=2";

            request.open('GET', url);
            request.send();
            pageCount ++;
        }
    };


    /* eslint-disable */
    export default {
        name: 'News',
        data() {
            return {
                news : contents,
                msg: 'Welcome to Your Vue.js App'
            }
        },
        methods: {
            getPostTime: function(date){
                let tmpTime = date.toString();
                let publishDate = new Date(date);
                let time = ((new Date()).getTime() - publishDate.getTime())/3600000;
                let strTime = "";
                if(Math.round(time) == 0){
                    strTime = Math.round(time*59) == 1 ? Math.round(time*60) + " minute ago ..." : Math.round(time*60) + " minutes ago ..." ;
                }
                else strTime = Math.round(time) == 1 ? Math.round(time) + " hour ago ..." : Math.round(time) + " hours ago ...";
                return strTime;
            },
            getShortDescription: function(description){
                if(description != null && description.length > 105) {
                    return description.substring(0, 104) + ' ...';
                }
                return description;
            }
        }
    }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .card-title {
        font-size: 18px;
        font-weight: bold;
        min-height: 60px;
    }
    a.card:hover {
        text-decoration: none; 
    }
</style>
