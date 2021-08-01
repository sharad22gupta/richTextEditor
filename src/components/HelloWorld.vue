<template>
  <div>
    <div v-if="editor">
      <button
        @click="
          editor
            .chain()
            .focus()
            .toggleBold()
            .run()
        "
        class="btn"
        :class="{ 'is-active': editor.isActive('bold') }"
      >
        <i class="fas fa-bold"></i>
      </button>
      <button
        @click="
          editor
            .chain()
            .focus()
            .toggleItalic()
            .run()
        "
        class="btn"
        :class="{ 'is-active': editor.isActive('italic') }"
      >
        <i class="fas fa-italic"></i>
      </button>
      <button
        @click="
          editor
            .chain()
            .focus()
            .toggleUnderline()
            .run()
        "
        class="btn"
        :class="{ 'is-active': editor.isActive('underline') }"
      >
        <i class="fas fa-underline"></i>
      </button>
      <button
        @click="setLink"
        class="btn"
        :class="{ 'is-active': editor.isActive('link') }"
      >
        <i class="fas fa-link"></i>
      </button>
      <button
        @click="
          editor
            .chain()
            .focus()
            .unsetLink()
            .run()
        "
        class="btn"
        v-if="editor.isActive('link')"
      >
        <i class="fas fa-unlink"></i>
      </button>
      <button @click="addImage" class="btn">
        <i class="far fa-images"></i>
      </button>
    </div>
    <editor-content :editor="editor" />
  </div>
</template>

<script>
import { Editor, EditorContent } from "@tiptap/vue-2";
import StarterKit from "@tiptap/starter-kit";
import Underline from "@tiptap/extension-underline";
import Link from "@tiptap/extension-link";
import Image from "@tiptap/extension-image";
import VueComponent from "./Extension.js";
export default {
  components: {
    EditorContent,
  },

  data() {
    return {
      editor: null,
    };
  },

  mounted() {
    this.editor = new Editor({
      extensions: [StarterKit, Underline, Link, Image, VueComponent],
      content: `
        <h2>
          Hi there,
        </h2>
        
        <p>
          I know, I know, this is impressive. It’s only the tip of the iceberg though. Give it a try and click a little bit around.
        </p>
        <vue-component count="0"></vue-component>
      `,
    });
  },
  methods: {
    setLink() {
      const url = window.prompt("URL");

      this.editor
        .chain()
        .focus()
        .extendMarkRange("link")
        .setLink({ href: url })
        .run();
    },
    addImage() {
      const url = window.prompt("URL");

      if (url) {
        this.editor
          .chain()
          .focus()
          .setImage({ src: url })
          .run();
      }
    },
  },
  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>
<style>
.btn {
  margin: 5px;
}
</style>
