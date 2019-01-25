<template>
	<div class="editor">
		<h1>エディター画面</h1>
		<span>{{ user.displayName }}</span>
		<button @click="logout">ログアウト</button>
		<div class="editorWrapper">
			<div class="memoListWrapper">
				<div
					class="memoList"
					v-for="(memo, index) in memos"
					:key="index"
					@click="selectMemo(index)"
					:data-selected="index == selectedIndex"
				>
					<p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
				</div>
			</div>
			<textarea class="marksdown" v-model="markdown"></textarea>
			<div class="preview" v-html="preview()"></div>
		</div>
	</div>
</template>
<script>
import marked from "marked";

export default {
	name: "editor",
	props: ["user"],
	data() {
		return {
			memos: [
				{
					markdown: ""
				}
			],
			selectedIndex: 0
		}
	},
	methods: {
		logout() {
			firebase.auth().signOut();
		},
		addMemo() {
			this.memos.push({
				markdown: "無題のメモ"
			});
		},
		selectMemo(index) {
			this.selectedIndex = index;
		},
		preview() {
			return marked(this.memos[this.selectedIndex].markdown);
		},
		displayTitle(text) {
			return text.split(/\n/)[0];
		}
	}
}
</script>

<style lang="scss" scoped>
.editorWrapper {
	display: flex;
}
.markdown {
	width: 50%;
	height: 500px;
}
.preview {
	width: 50%;
	text-align: left;
}
</style>
