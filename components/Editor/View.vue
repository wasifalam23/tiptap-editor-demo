<script setup>
	import { EditorContent, Editor } from "@tiptap/vue-3";
	import StarterKit from "@tiptap/starter-kit";
	const { output } = defineProps(["output"]);

	const editor = ref(null);
	const editable = ref(false);

	onMounted(() => {
		editor.value = new Editor({
			editable: editable.value,
			content: output,
			extensions: [StarterKit],
		});
	});

	watch(editable, () => {
		editor.value.setEditable(editable);
	});

	onBeforeUnmount(() => {
		editor.value = null;
	});
</script>

<template>
	<div class="editor mt-8">
		<div class="checkbox">
			<input type="checkbox" id="editable" v-model="editable" />
			<label for="editable">editable</label>
		</div>
		<editor-content :editor="editor" />
	</div>
</template>

<style lang="scss">
	/* Basic editor styles */
	.tiptap {
		> * + * {
			margin-top: 0.75em;
		}
	}

	.checkbox {
		margin-bottom: 1rem;

		input[type="checkbox"] {
			margin-right: 0.5rem;
		}
	}

	[contenteditable="false"] {
		cursor: not-allowed;
	}
</style>
