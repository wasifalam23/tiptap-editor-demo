<script setup>
	import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
	import {
		faBold,
		faItalic,
		faStrikethrough,
		faParagraph,
		faListUl,
	} from "@fortawesome/free-solid-svg-icons";

	import { EditorContent, Editor } from "@tiptap/vue-3";
	import StarterKit from "@tiptap/starter-kit";
	import Bold from "@tiptap/extension-bold";
	import Italic from "@tiptap/extension-italic";
	import BulletList from "@tiptap/extension-bullet-list";
	import ListItem from "@tiptap/extension-list-item";
	import Strike from "@tiptap/extension-strike";

	import Document from "@tiptap/extension-document";
	import Paragraph from "@tiptap/extension-paragraph";
	import Text from "@tiptap/extension-text";
	import { generateHTML } from "@tiptap/html";

	const editor = ref(null);
	const json = ref({
		type: "doc",
		content: [],
	});

	const output = computed(() => {
		return generateHTML(json.value, [
			Document,
			Paragraph,
			Text,
			Bold,
			Italic,
			ListItem,
			BulletList,
			Strike,
		]);
	});

	const submitHandler = () => {
		console.log(editor.value.getJSON());
		const jsonValue = editor.value.getJSON();
		json.value = jsonValue;
		console.log(output.value);
	};

	const resetHandler = () => {
		json.value = {
			type: "doc",
			content: [],
		};
	};

	onMounted(() => {
		editor.value = new Editor({
			editorProps: {
				attributes: {
					class:
						"prose prose-sm sm:prose lg:prose-lg xl:prose-2xl mx-auto focus:outline-none h-52 px-2 py-2",
				},
			},
			extensions: [StarterKit],
		});
	});

	onBeforeUnmount(() => {
		editor.value = null;
	});
</script>

<template>
	<main class="max-w-2xl mx-auto mt-14">
		<h2 class="mb-4 text-lg text-cyan-800 font-medium">Tiptap Editor Demo</h2>
		<div class="border border-gray-400 rounded-md">
			<div
				v-if="editor"
				class="flex gap-4 pl-5 py-2 px-1 bg-slate-300 items-center">
				<button
					@click="editor.chain().focus().toggleBold().run()"
					:disabled="!editor.can().chain().focus().toggleBold().run()">
					<FontAwesomeIcon
						:class="{ 'text-blue-600': editor.isActive('bold') }"
						:icon="faBold" />
				</button>
				<button
					@click="editor.chain().focus().toggleItalic().run()"
					:disabled="!editor.can().chain().focus().toggleItalic().run()">
					<FontAwesomeIcon
						:class="{ 'text-blue-600': editor.isActive('italic') }"
						:icon="faItalic" />
				</button>
				<button
					@click="editor.chain().focus().toggleStrike().run()"
					:disabled="!editor.can().chain().focus().toggleStrike().run()">
					<FontAwesomeIcon
						:class="{ 'text-blue-600': editor.isActive('strike') }"
						:icon="faStrikethrough" />
				</button>

				<button @click="editor.chain().focus().setParagraph().run()">
					<FontAwesomeIcon
						:class="{ 'text-blue-600': editor.isActive('paragraph') }"
						:icon="faParagraph" />
				</button>

				<button @click="editor.chain().focus().toggleBulletList().run()">
					<FontAwesomeIcon
						:class="{ 'text-blue-600': editor.isActive('bulletList') }"
						:icon="faListUl" />
				</button>
			</div>
			<editor-content :editor="editor" />
		</div>

		<div class="flex w-full justify-between mt-2 gap-2">
			<button
				@click="submitHandler"
				class="bg-yellow-400 font-medium rounded-md w-full px-2 py-2 hover:bg-yellow-500">
				Save Template
			</button>
			<button
				@click="resetHandler"
				class="bg-gray-300 w-full px-2 rounded-md py-2 hover:bg-gray-400">
				Cancel
			</button>
		</div>

		<EditorView :output="output" />
	</main>
</template>

<style lang="scss">
	/* Basic editor styles */
	ul,
	ol {
		padding: 0 1rem;
		list-style: initial !important;
	}
	.tiptap {
		> * + * {
			margin-top: 0.75em;
		}

		h1,
		h2,
		h3,
		h4,
		h5,
		h6 {
			line-height: 1.1;
		}

		code {
			background-color: rgba(#616161, 0.1);
			color: #616161;
		}

		pre {
			background: #0d0d0d;
			color: #fff;
			font-family: "JetBrainsMono", monospace;
			padding: 0.75rem 1rem;
			border-radius: 0.5rem;

			code {
				color: inherit;
				padding: 0;
				background: none;
				font-size: 0.8rem;
			}
		}

		img {
			max-width: 100%;
			height: auto;
		}

		blockquote {
			padding-left: 1rem;
			border-left: 2px solid rgba(#0d0d0d, 0.1);
		}

		hr {
			border: none;
			border-top: 2px solid rgba(#0d0d0d, 0.1);
			margin: 2rem 0;
		}
	}
</style>
