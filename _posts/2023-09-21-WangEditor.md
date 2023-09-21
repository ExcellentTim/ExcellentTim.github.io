---
layout: post
title: WangEditor
renderNumberedHeading: true
grammar_cjkRuby: true
date: 2023-09-21 17:00:00
---

``` xml
<template>
  <div>
    <div class="w-e-for-vue" @click="handleClickFocus">
      <!-- 工具栏 -->
      <el-affix :offset="0">
        <Toolbar class="w-e-for-vue-toolbar" :editor="editor" :defaultConfig="toolbarConfig"> </Toolbar>
      </el-affix>

      <!-- 编辑器 -->
      <WEditor
        class="w-e-for-vue-editor"
        :disabled="isDisabled"
        :defaultConfig="editorConfig"
        v-model="content"
        @onChange="onChange"
        @onCreated="onCreated"
      >
      </WEditor>
    </div>
  </div>
</template>

<script>
import { Editor as WEditor, Toolbar } from '@wangeditor/editor-for-vue';
// import { DomEditor } from '@wangeditor/editor';

export default {
  name: 'wangEditor',
  components: { WEditor, Toolbar },
  props: ['editorParams'],
  data() {
    return {
      editor: null, // 富文本编辑器对象
      content: null, // 富文本内容
      placeholder: null, // 富文本占位内容
      uploadImageUrl: null, // 富文本上传图片的地址
      isDisabled: false, // 富文本是否禁用

      // 工具栏配置
      toolbarConfig: {
        // toolbarKeys: [], // 显示指定的菜单项
        excludeKeys: [
          'code',
          'emotion', // 表情
          'insertLink', // 链接
          'group-image', // 图片
          'group-video', // 视频
          'insertTable', // 表格
          'codeBlock', // 代码块
        ], // 隐藏指定的菜单项
      },

      // 编辑器配置
      editorConfig: {
        placeholder: '请输入内容......',
        MENU_CONF: ['uploadImage'],
      },
    };
  },
  watch: {
    /**
     * 深度监听富文本参数
     */
    editorParams: {
      handler: function (newVal, oldVal) {
        if (newVal != null) {
          console.log('new', { newVal, oldVal });
          this.content = newVal.content;
          this.editorConfig.placeholder = this.placeholder;
          this.uploadImageUrl = newVal.uploadImageUrl;
          this.setUploadImageConfig();
          this.isDisabled = newVal.isDisabled;
        }
      },
      immediate: true,
      deep: true,
    },
  },
  methods: {
    /**
     * 实例化富文本编辑器
     * 文档链接：https://www.wangeditor.com/
     */
    onCreated(editor) {
      this.editor = Object.seal(editor);
      this.setIsDisabled();
    },

    /**
     * 监听富文本编辑器
     */
    onChange(editor) {
      // console.log('onChange =>', editor.getHtml());
      this.$emit('change', editor.getHtml());
      // const toolbar = DomEditor.getToolbar(this.editor);
      // console.log('toolbar.getConfig().toolbarKeys', toolbar.getConfig().toolbarKeys);
    },
    /**
     * 点击聚焦
     */
    handleClickFocus() {
      this.editor.focus();
    },
    /**
     * this.editor.getConfig().spellcheck = false
     * 由于在配置中关闭拼写检查，值虽然设置成功，但是依然显示红色波浪线
     * 因此在富文本编辑器组件挂载完成后，操作 Dom 元素设置拼写检查 spellcheck 为假
     */
    async setSpellCheckFasle() {
      let doms = await document.getElementsByClassName('w-e-scroll');
      for (let vo of doms) {
        if (vo) {
          if (vo.children.length > 0) {
            vo.children[0].setAttribute('spellcheck', 'false');
          }
        }
      }
    },

    /**
     * 设置富文本是否禁用
     */
    async setIsDisabled() {
      if (this.editor) {
        this.isDisabled ? this.editor.disable() : this.editor.enable();
      }
    },

    /**
     * 上传图片配置
     */
    setUploadImageConfig() {
      this.editorConfig.placeholder = this.placeholder;
      this.editorConfig.MENU_CONF['uploadImage'] = {
        fieldName: 'files', // 文件字段名， 默认值 'wangeditor-uploaded-image'
        maxFileSize: 1 * 1024 * 1024, // 单个文件的最大体积限制，默认为 2M，此次设置为 1M
        maxNumberOfFiles: 10, // 最多可上传几个文件，默认为 100
        allowedFileTypes: ['image/*'], // 选择文件时的类型限制，默认为 ['image/*'] ，若不想限制，则设置为 []
        meta: {
          // 自定义上传参数，例如传递验证的 token 等，参数会被添加到 formData 中，一起上传到服务端
          token: 'xxx',
          otherKey: 'yyy',
        },
        metaWithUrl: false, // 将 meta 拼接到 URL 参数中，默认 false
        headers: {
          // 设置 HTTP 请求头信息
          // ...
        },
        server: this.uploadImageUrl, // 上传图片接口地址
        withCredentials: false, // 跨域是否传递 cookie ，默认为 false
        timeout: 5 * 1000, // 超时时间，默认为 10 秒，此次设置为 5 秒

        // 上传之前触发
        onBeforeUpload(file) {
          return file;
        },

        // 上传进度的回调函数
        onProgress(progress) {
          console.log('progress', progress);
        },

        // 单个文件上传成功之后
        onSuccess(file, res) {
          console.log(`${file.name} 上传成功`, res);
        },

        // 单个文件上传失败
        onFailed(file, res) {
          console.log(`${file.name} 上传失败`, res);
        },

        // 上传错误，或者触发 timeout 超时
        onError(file, err, res) {
          console.log(`${file.name} 上传出错`, err, res);
        },
      };
    },

    /**
     * 获取编辑器文本内容
     */
    getEditorText() {
      const editor = this.editor;
      if (editor != null) {
        return editor.getText();
      }
    },

    /**
     * 获取编辑器Html内容
     */
    getEditorHtml() {
      const editor = this.editor;
      if (editor != null) {
        return editor.getHtml();
      }
    },
  },
  created() {},
  mounted() {
    this.setSpellCheckFasle(); // 设置拼写检查 spellcheck 为假
    document.activeElement.blur(); // 取消富文本自动聚焦且禁止虚拟键盘弹出
  },
  /**
   * 销毁富文本编辑器
   */
  beforeDestroy() {
    const editor = this.editor;
    if (editor != null) {
      editor.destroy();
    }
  },
};
</script>

<style src="@wangeditor/editor/dist/css/style.css"></style>

<style lang="scss" scoped>
.w-e-full-screen-container {
  z-index: 99;
}

.w-e-for-vue {
  margin: 0;
  // border: 1px solid #ccc;
  cursor: text;
  :deep(.w-e-for-vue-toolbar) {
    // border-bottom: 1px solid #ccc;

    .w-e-toolbar {
      background-color: #fafafa;
    }
  }

  .w-e-for-vue-editor {
    height: auto;

    :deep(.w-e-text-container) {
      min-height: 500px;
      .w-e-text-placeholder {
        top: 6px;
        color: #666;
      }

      pre {
        code {
          text-shadow: unset;
        }
      }

      p {
        margin: 5px 0;
        font-size: 14px; // 设置编辑器的默认字体大小为14px
      }
    }
  }
}
</style>
```

