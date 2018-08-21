<template>
	<div class="container">
		<SearchBar @termChange="onTermChange"></SearchBar>
		<div class="row">
			<div class="col-md-8">
				<VideoDetail :video="selectedVideo"></VideoDetail>
			</div>
			<div class="col-md-4">
				<VideoList :videos="videos" @videoSelect="onVideoSelect"></VideoList>
			</div>
		</div>

	</div>
</template>

<script>
import axios from 'axios';
import VideoDetail from './components/VideoDetail';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
const API_KEY = 'AIzaSyAOel71cpquuVNMQbwmVrC5MMjPc-uHscY';

export default {
	name : 'App',
	data(){
		return {
			videos :[],
			selectedVideo : null
		};
	},
	components : {
		SearchBar: SearchBar,
		VideoList: VideoList,
		VideoDetail:VideoDetail
	},
	methods:{
		onTermChange(searchTerm){
			axios.get('https://www.googleapis.com/youtube/v3/search',{
				params:{
					key:API_KEY,
					type:'video',
					part: 'snippet',
					q: searchTerm
				}
			})
			.then(response => {
				this.videos = response.data.items;
			});
			
		},

		onVideoSelect(video){
			this.selectedVideo = video;
		}
	}
}

</script>