<template>
	<section class="jumbotron">
		<h3 class="jumbotron-heading">Search Github Users</h3>
		<div>
			<input
				type="text"
				placeholder="enter the name you search"
				v-model="keyword" />&nbsp;
			<button @click="searchUsers">Search</button>
		</div>
	</section>
</template>

<script>
	import axios from 'axios';
	export default {
		name: 'Search',
		data() {
			return {
				keyword: '',
			};
		},
		methods: {
			searchUsers() {
				const searchUrl = new URL(
					'https://api.github.com/search/users'
				);
				const searchParams = new URLSearchParams({
					q: this.keyword,
				});
				searchUrl.search = searchParams;

				this.$bus.$emit('updateListData', {
					isFirst: false,
					isLoading: true,
					errMsg: '',
				});
				axios
					.get(
						// `https://api.github.com/search/users?q=${this.keyword}`
						searchUrl.href
					)
					.then(rsp => {
						this.$bus.$emit('updateListData', {
							users: rsp.data.items,
							isLoading: false,
							errMsg: '',
						});
					})
					.catch(err => {
						this.$bus.$emit('updateListData', {
							users: [],
							isLoading: false,
							errMsg: err.message,
						});
					});
			},
		},
	};
</script>

<style></style>
