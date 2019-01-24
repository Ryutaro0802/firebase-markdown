<template>
	<div class="editor">
		<h1>エディター画面</h1>
		<span>{{ user.displayName }}</span>
		<button @click="logout">ログアウト</button>
		<div class="editorWrapper">
			<textarea class="marksdown" v-model="markdown"></textarea>
			<div class="preview" v-html="preview()"></div>
		</div>
	</div>
</template>
<script>
console.log("editor");
import marked from "marked";

export default {
	name: "editor",
	props: ["user"],
	data() {
		return {
            marked: ""
		}
	},
	methods: {
		logout() {
			firebase.auth().signOut();
        },
        preview() {
            return marked(this.markdown);
        }
	}
}
</script>

<style lang="scss" scoped>
    .editorWrapper {
        width: 50%;
        height: 500px;
    }
    .preview {
        width: 50%;
        text-align: left;
    }
</style>
