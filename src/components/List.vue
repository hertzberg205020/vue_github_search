<template>
	<div class="row">
		<!--展示數據-->
		<div
			class="card"
			v-show="info.users.length > 0"
			v-for="user in info.users"
			:key="user.login">
			<a
				:href="user.html_url"
				target="_blank">
				<img
					:src="user.avatar_url"
					style="width: 100px" />
			</a>
			<p class="card-text">{{ user.login }}</p>
		</div>
		<!--展現歡迎詞-->
		<h1 v-show="info.isFirst">歡迎使用</h1>
		<!--展現加載中-->
		<h1 v-show="info.isLoading">加載中...</h1>
		<!--展現錯誤消息-->
		<h1 v-show="info.errMsg">請求出錯了，請稍後重試</h1>
	</div>
</template>

<script>
	export default {
		name: 'List',
		data() {
			return {
				info: {
					users: [],
					isFirst: true,
					isLoading: false,
					errMsg: '',
				},
			};
		},
		mounted() {
			this.$bus.$on('updateListData', data => {
				// 後面物件的屬性會覆蓋前面物件的屬性，
				// 若前面物件沒有該屬性，則會新增該屬性
				this.info = { ...this.info, ...data };
			});
		},
		beforeDestroy() {
			this.$bus.$off('updateListData');
		},
	};
</script>

<style scoped>
	.album {
		min-height: 50rem; /* Can be removed; just added for demo purposes */
		padding-top: 3rem;
		padding-bottom: 3rem;
		background-color: #f7f7f7;
	}

	.card {
		float: left;
		width: 33.333%;
		padding: 0.75rem;
		margin-bottom: 2rem;
		border: 1px solid #efefef;
		text-align: center;
	}

	.card > img {
		margin-bottom: 0.75rem;
		border-radius: 100px;
	}
</style>
