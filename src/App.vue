<template >
    <v-app  >



        <v-app-bar app id="searchBar2" flat dark height="108">
            <v-row class="align-center justify-center">
                <!--        <div class="display-1 font-weight-bold elevation-2"><span style="color:#00e9ff;">M</span><span style="color:#16d2ff;">o</span><span style="color:#2cbaff;">v</span><span style="color:#41a3ff;">i</span><span style="color:#578cff;">e </span><span style="color:#6d74ff;"> </span><span style="color:#835dff;">S</span><span style="color:#9946ff;">c</span><span style="color:#ae2fff;">o</span><span style="color:#c417ff;">r</span><span style="color:#da00ff;">e</span><span style="color:#de19e6;">s </span><span style="color:#e132cc;"> </span><span style="color:#e54ab3;">a</span><span style="color:#e96399;">n</span><span style="color:#ec7c80;">d </span><span style="color:#f09566;"> </span><span style="color:#f4ae4d;">I</span><span style="color:#f8c633;">n</span><span style="color:#fbdf1a;">f</span><span style="color:#fff800;">o</span></div><div></div><div></div>-->
              <div style="text-align: start; " >
                <v-toolbar-title class="ma-3 display-1 font-weight-bold d-none d-lg-flex " style="color: #FFFFFF" >Movie Scores and Info <br> </v-toolbar-title>
                <a style="font-size: large; margin-left: 15px">For testing purposes (limited API requests)</a>
              </div>

                <v-btn @click="getFirst(popular), hideDiv (), hideDiv2 (),showTitluP(),hideTitluTR()" style="background-color: #2A5BCC" outlined class="mx-2 d-none d-lg-flex" >Popular</v-btn>
                <v-btn @click="getFirst(topRated), hideDiv(), hideDiv2 (),showTitluTR(),hideTitluP()" style="background-color: #2A5BCC" outlined class="d-none d-lg-flex">Top Rated</v-btn>

                <v-layout mt-2 class="d-flex " style="width: 200px">
                    <v-form style="background: none; width:350px;  margin-left: 20px;"  v-on:submit.prevent="getResult(query),showDiv2()">
                        <v-text-field  placeholder="Type In Your Search" clearable dense flat hide-details class="mb-2"
                                       append-icon="mdi-magnify"  color="black" solo-inverted
                                       @click:append.prevent="getResult(query),showDiv2()" v-model="query"
                        ></v-text-field>
                    </v-form>
                </v-layout>


                <v-col cols="4"  class="mt-1 d-none d-lg-flex" >
                            <v-card  style="background: none" flat>
                    <v-row style="align-items: center; " >
                        <div id="Compair" :key="localMovie.imdbID" v-for="(localMovie,i) in myLocalData"  style=" width: 55px; margin-bottom: -4px" >
                            <img :src="localMovie.posterDeSus.Poster" @click="removeFromLocal(i)" style="max-width: 50px" >
                        </div>
                    </v-row>
                    <v-row style="align-items: center; color: white" >
                        <div id="CompairNota" :key="localNota.imdbVotes"  v-for="localNota in myLocalData" style="width: 55px">
                            {{parseFloat(localNota.notaDeSus).toFixed(2)}}
                        </div>
                    </v-row>
                            </v-card>
                </v-col>
                <v-card >
<!--                    <img src="./fry.png" alt="">-->
                </v-card>

            </v-row>
        </v-app-bar>



        <v-content style="background: #ffffff"   class="">

            <v-container id="RandSus" fluid class="px-10 pb-6" style="display: none; " >
                <v-row id="results" class="d-inline-flex py-0" >
                    <v-card :key="movie.imdbID" v-for="movie in results" style="background: #e8ebf9;  " class="mx-2 mt-3 "  ripple dark elevation="3">
                        <v-col class="ma-2 mb-0 pa-0 justify-center" >

                            <img :src="movie.Poster" alt="" style="width: 150px; max-height: 240px; " v-on:click="getResulttwo(movie.imdbID), showDiv()"  >
                            <div style="text-align: center; width: 150px; color: black">
                                {{movie.Title}}
                                ({{movie.Year}})
                            </div>
                        </v-col>
                    </v-card>
                </v-row>
            </v-container>




                <div id="Note" style="display: none" class="px-12">     <!-- style="background: #e8ebf9" elevation="8"-->
                  <v-card    dark  class="mx-auto mb-5 " justify="center" raised max-width="800"  style="background: #e8ebf9;  " ripple  >
            <v-container class="pa-0" >


                    <v-row class="align-center justify-center  " style="color: black">
                        <v-card-title class="display-1 font-weight-bold ml-n4 justify-center " style="width: 800px">{{Info.Title}} </v-card-title><br/>
                    </v-row>

                    <v-row justify="center">

                        <div id="cuPoster" class="mx-2 "  >
                            <img :src="Info.Poster" v-on:click="fromLocal(), showDiv()">
                        </div>

                        <v-col  cols="7" class="align-center py-0" justify-self="center" >
                            <v-card  flat  tile style="background: none; color: black">
                                <span>  {{Info.Plot}}  </span><br/>
                                <span style="font-weight: bold" > Year : </span><span>{{ Info.Year}}</span><br>
                                <span style="font-weight: bold"> Genre : </span><span>  {{Info.Genre}}  </span><br/>
                                <span style="font-weight: bold"> Runtime  : </span><span>{{Info.Runtime}}  </span><br/>
                                <span style="font-weight: bold"> Director  : </span><span>{{Info.Director}}  </span><br/>
                                <span style="font-weight: bold"> Writer  : </span><span>{{Info.Writer}}  </span><br/>
                                <span style="font-weight: bold"> Actors  : </span><span >{{Info.Actors}}  </span><br/>
                                <a style="font-weight: bold; color: #f3ce13" v-bind:href="'https://www.imdb.com/title/' + Info.imdbID + '/?ref_=fn_al_tt_2'" > ImdbRating  :</a><span> {{Info.imdbRating}} /10 </span><br/>
                                <a style="font-weight: bold; color: #E53935" v-bind:href="'https://www.rottentomatoes.com/search/?search=' + Info.Title"> Tomatometer :</a><span> {{NotaRT}} /100</span><br/>
                                <a style="font-weight: bold;color: #546E7A" v-bind:href="'https://www.metacritic.com/search/all/' + Info.Title + '/results'" > Metascore  :</a><span> {{NotaMetascore}} /100</span><br/>
                                <span id="Nota" class="title" style="color: #2A5BCC" > Average : {{parseFloat(NotaMea).toFixed(2)}} /100</span><br/>
                            </v-card>
                        </v-col>
                    </v-row>
            </v-container>
                  </v-card>
                </div>


            <v-card-title id="TitluP" class="py-5 pl-10" style="background-color: #2A5BCC; color: #FFFFFF">Popular</v-card-title>
            <v-card-title id="TitluTR" class="py-5 pl-10" style="display: none;background-color: #2A5BCC; color: #FFFFFF">Top Rated</v-card-title>

            <v-container  fluid class="pl-10 py-0 mb-4" >
                <v-row  class="d-inline-flex py-0 " >
                    <v-card  id="one" v-for="movieOne in resultsOne" :key="movieOne.id" class="mx-2 mt-3"  elevation="3" dark style="background: #e8ebf9" ripple>

                        <v-col class="ma-2 mb-0 pa-0" style="color: black">

                            <img :src="parteaUnu + movieOne.poster_path" style="max-width: 150px" v-on:click="getResult(movieOne.title),showDiv2()" alt="">
                            <div style="text-align: center; max-width: 150px">
                                {{movieOne.title}}
                            </div>

                        </v-col>

                    </v-card>
                </v-row>
            </v-container>





        </v-content>
    </v-app>
</template>

<script>/* eslint-disable no-console */
import axios from 'axios';
export default {
    name: 'App',
    components: { },
    data() {
        return {
            msg: 'Search',
            query: '',
            results: [],
            Info: '',
            NotaIMDB: '',
            NotaMetascore: '',
            NotaRT: '',
            NotaMea: [],
            myLocalData: [],
            myLocalNota: [],
            resultsOne:[],
            parteaUnu: 'https://image.tmdb.org/t/p/w500',
            popular:'popular',
            topRated:'top_rated',
            selectedMovieData:[]
        }
    },
    methods: {
        getFirst(popular) {
            axios.get('https://api.themoviedb.org/3/movie/' + popular + '?api_key=8ed724260519b16c57ff724018878b40&language=en-US&page=1')
                .then(response => {
                    this.resultsOne = [];
                    for (let i = 0; i < response.data.results.length; i++) {
                        let movieOne = response.data.results[i];
                        this.resultsOne.push(movieOne);
                    }
                });
        },
        getResult(query) {
            axios.get('http://www.omdbapi.com/?s=' + query + '&apikey=1229d4bb')
                .then(response => {
                    this.results = [];
                    for (let i = 0; i < response.data.Search.length; i++) {
                        let movie = response.data.Search[i];
                        this.results.push(movie);
                    }
                });
        },
        getResulttwo(imdb) {
            this.NotaMea = [];
            axios.get('http://www.omdbapi.com/?i=' + imdb + '&apikey=1229d4bb')
                .then(response => {
                    this.Info = response.data;
                    localStorage.setItem(this.Info.imdbID, JSON.stringify(this.Info));

                    if (response.data.imdbRating.includes('N/A')) {
                        this.NotaIMDB = 'Unavailable';
                    } else {
                        this.NotaIMDB = parseFloat(response.data.imdbRating * 10);
                        this.NotaMea.push(this.NotaIMDB);
                    }

                    if (response.data.Metascore.includes('N/A')) {
                        this.NotaMetascore = 'Unavailable';
                    } else {
                        this.NotaMetascore = parseFloat(response.data.Metascore);
                        this.NotaMea.push(this.NotaMetascore)
                    }

                    if (response.data.Ratings.length > 1) {
                        if (response.data.Ratings[1].Source === 'Rotten Tomatoes') {
                            this.NotaRT = parseFloat(response.data.Ratings[1].Value);
                            this.NotaMea.push(this.NotaRT)
                        } else {
                            this.NotaRT = 'Unavailable';
                        }
                    } else {
                        this.NotaRT = 'Unavailable';
                    }


                    let sum = this.NotaMea.reduce((previous, current) => current += previous);
                    this.NotaMea = sum / this.NotaMea.length;
                    localStorage.setItem(this.Info.imdbVotes, (this.NotaMea))
                })
        },
        showDiv() {
            document.getElementById('Note').style.display = "block";
        },
        showDiv2() {
            document.getElementById('RandSus').style.display = "block";
        },
        showTitluP() {
            document.getElementById('TitluP').style.display = "block";
        },
        showTitluTR() {
            document.getElementById('TitluTR').style.display = "block";
        },
        hideTitluP() {
            document.getElementById('TitluP').style.display = "none";
        },
        hideTitluTR() {
            document.getElementById('TitluTR').style.display = "none";
        },
        hideDiv() {
            document.getElementById('Note').style.display = "none";
        },
        hideDiv2() {
            document.getElementById('RandSus').style.display = "none";
        },
        fromLocal() {
          const movieToAddImdbID = this.Info.imdbID;
          if (this.isMovieInfoInLocalData(movieToAddImdbID)) {
            return
          }
          if (this.myLocalData.length<7) {
            let movieDeSus = {
              posterDeSus: undefined,
              notaDeSus: undefined
            };
            movieDeSus.posterDeSus = JSON.parse(localStorage.getItem(this.Info.imdbID));
            movieDeSus.notaDeSus = parseFloat(JSON.parse(localStorage.getItem(this.Info.imdbVotes)));
            this.myLocalData.push(movieDeSus);
            // console.log(this.myLocalData)
            // this.myLocalData.push(JSON.parse(localStorage.getItem(this.Info.imdbID)));
            // this.myLocalNota.push(JSON.parse(localStorage.getItem(this.Info.imdbVotes)));
          }

          else {
            let indexOfSmallestGrade = 0;
            for (let i = 0; i < this.myLocalData.length; i++) {
              if(this.myLocalData[indexOfSmallestGrade].notaDeSus > this.myLocalData[i].notaDeSus){
                indexOfSmallestGrade = i
              }
            }
            // remove element with the smallest grade from array
            this.myLocalData.splice(indexOfSmallestGrade, 1);
            this.fromLocal()
            // alert("Maximum movies to compair is 7")alNota.push(JSON.parse(localStorage.getItem(this.Info.imdbVotes)));
            }
        },
        removeFromLocal(i) {
          this.myLocalData.splice(i, 1)
        },
        isMovieInfoInLocalData(imdbID) {
            for (let i = 0; i < this.myLocalData.length; i++) {
                const movieInfo = this.myLocalData[i].posterDeSus;
                if (movieInfo.imdbID === imdbID)
                    return true;
                // console.log(`movieInfo[${i}]=`,movieInfo)
                // concatenare de stringuri
            }
            return false
        },
    },
    created(){
        this.getFirst('popular');
    },

}
</script>
<style>

    #Nota {
        /*color: rgb(255, 194, 60);*/
        /*color: linear-gradient(90deg, rgba(255, 194, 60, 1) 0%, rgba(194, 29, 253, 1) 52%, rgba(69, 245, 252, 1) 100%);*/
    }
    #searchBar2 {
        /*background: rgb(255, 194, 60);*/
        /*background: linear-gradient(90deg, rgba(255, 194, 60, 1) 0%, rgba(194, 29, 253, 1) 52%, rgba(69, 245, 252, 1) 100%);*/
        /*background: cornflowerblue;*/
        background: #2A5BCC;
    }
</style>