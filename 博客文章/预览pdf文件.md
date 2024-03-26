使用插件:  **vue-pdf-embed**

### **代码参考**

```
<template>
	<div class="pdf-preview">
		<div class="page-tool">
			<div class="page-tool-item" @click="lastPage">上一页</div>
			<div class="page-tool-item" @click="nextPage">下一页</div>
			<div class="page-tool-item">{{state.pageNum}}/{{state.numPages}}</div>
			<div class="page-tool-item" @click="pageZoomOut">放大</div>
			<div class="page-tool-item" @click="pageZoomIn">缩小</div>
			<div class="page-tool-item"  @click="download">下载</div>

		</div>
		<div class="pdf-wrap">
			<vue-pdf-embed :source="state.source"  :style="scale"  class="vue-pdf-embed" :page="state.pageNum" />
		</div>

	</div>
</template>
<script setup lang="ts">
import { reactive, onMounted, computed } from "vue";
import VuePdfEmbed from "vue-pdf-embed";
import { createLoadingTask } from "vue3-pdfjs"; // 获得总页数 vue3-pdfjs/esm
import { saveAs } from 'file-saver';

const props = defineProps({
	pdfUrl: {
		type: String,
		required: true
	}
})
const state = reactive({
	source: props.pdfUrl,
	pageNum: 1,
	scale: 1, // 缩放比例
	numPages: 0, // 总页数
});

const scale = computed(() => `transform:scale(${state.scale})`)

const lastPage =() => {
	if (state.pageNum > 1) {
		state.pageNum -= 1;
	}
}
const nextPage =() => {
	if (state.pageNum < state.numPages) {
		state.pageNum += 1;
	}
}
const  pageZoomOut = () =>{
	console.log(111)
	if (state.scale < 2) {
		state.scale += 0.1;
	}
}
const pageZoomIn =() => {
	if (state.scale > 1) {
		state.scale -= 0.1;
	}
}
const download  = ()=>{
	saveAs(props.pdfUrl, '下载.pdf');

}
onMounted(() => {
	const loadingTask = createLoadingTask(state.source);
	loadingTask.promise.then((pdf:{numPages: number}) => {
		state.numPages = pdf.numPages;
	});
});
</script>
<style lang="scss" scoped>
.pdf-preview {
	.vue-pdf-embed {
		box-sizing: border-box;
		width: 515px;
		margin: 0 auto;
		text-align: center;
		border: 1px solid #e5e5e5;
	}
	.page-tool {
		position: absolute;
		z-index: 100;
		display: flex;
		color: white;
		background: #333333;
		cursor: pointer;
		border-radius: 19px;
		.page-tool-item {
			padding: 8px 15px;
			padding-left: 10px;
			cursor: pointer;
		}
	}
}




</style>
```

具体api:



### Props

| Name               | Type              | Accepted values                                         | Description                                                  |
| ------------------ | ----------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| annotationLayer    | `boolean`         | `true` or `false`                                       | whether the annotation layer should be enabled               |
| height             | `number`          | natural numbers                                         | desired page height in pixels (ignored if the width property is specified) |
| imageResourcesPath | `string`          | URL or path with trailing slash                         | path for icons used in the annotation layer                  |
| page               | `number`          | `1` to the last page number                             | number of the page to display (displaying all pages if not specified) |
| rotation           | `number`          | `0`, `90`, `180`, `270` (multiples of `90`)             | desired page rotation angle in degrees                       |
| scale              | `number`          | rational numbers                                        | desired page viewport scale                                  |
| source             | `string` `object` | document URL or Base64 or typed array or document proxy | source of the document to display                            |
| textLayer          | `boolean`         | `true` or `false`                                       | whether the text layer should be enabled                     |
| width              | `number`          | natural numbers                                         | desired page width in pixels                                 |

### Events

| Name                  | Value                                                        | Description                                |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------ |
| internal-link-clicked | destination page number                                      | internal link was clicked                  |
| loaded                | PDF document proxy                                           | finished loading the document              |
| loading-failed        | error object                                                 | failed to load document                    |
| password-requested    | object with `callback` function and `isWrongPassword` flag   | password is needed to display the document |
| progress              | object with number of `loaded` pages along with `total` number of pages | tracking document loading progress         |
| rendered              | –                                                            | finished rendering the document            |
| rendering-failed      | error object                                                 | failed to render document                  |

### Slots

| Name        | Props                | Description                          |
| ----------- | -------------------- | ------------------------------------ |
| after-page  | `page` (page number) | content to be added after each page  |
| before-page | `page` (page number) | content to be added before each page |

### Public Methods

| Name     | Arguments                                                    | Description                          |
| -------- | ------------------------------------------------------------ | ------------------------------------ |
| download | filename (`string`)                                          | download document                    |
| print    | print resolution (`number`), filename (`string`), all pages flag (`boolean`) | print document via browser interface |



参考文档地址  =>  [**地址**](https://www.npmjs.com/package/vue-pdf-embed?activeTab=readme) 