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
				<button class="addMemoBtn" @click="addMemo">メモの追加</button>
				<button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">選択中のメモの削除</button>
				<button class="saveMemoBtn" @click="saveMemos">メモの保存</button>
			</div>
			<textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
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
	created() {
		firebase.database().ref("memos/" + this.user.uid).once("value")
			.then(result => {
				if (result.val()) {
					this.memos = result.val();
				}
			});
	},
	mounted() {
		console.log('mounted');
		document.addEventListener('keydown', this.keyDownHandler);
	},
	beforeDestroy() {
		console.log('destroy');
		document.removeEventListener('keydown', this.keyDownHandler);
	},
	methods: {
		logout() {
			firebase.auth().signOut();
		},
		keyDownHandler(e) {
			if (e.key === 's' && (e.metaKey || e.ctrlKey)) {
				e.preventDefault();
				console.log('save');
				this.saveMemos();
			}
		},
		addMemo() {
			this.memos.push({
				markdown: "無題のメモ"
			});
		},
		deleteMemo() {
			this.memos.splice(this.selectedIndex, 1);
			if (this.selectedIndex > 0) {
				this.selectedIndex--;
			}
		},
		saveMemos() {
			firebase.database().ref("memos/" + this.user.uid).set(this.memos);
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
.memoListWrapper {
	width: 20%;
	border-top: 1px solid #000;
}
.memoList {
	padding: 10px;
	box-sizing: border-box;
	text-align: left;
	border-bottom: 1px solid #000;
	&:nth-child(even) {
		background-color: #ccc;
	}
	&[data-selected="true"] {
		background-color: #ccf;
	}
}
.memoTitle {
	height: 1.5em;
	margin: 0;
	white-space: nowrap;
	overflow: hidden;
}
.addMemoBtn {
	margin-top: 20px;
	margin-bottom: 10px;
}
.deleteMemoBtn {
	margin: 10px;
}
.markdown {
	width: 40%;
	height: 500px;
}
.preview {
	width: 40%;
	text-align: left;
}
</style>
