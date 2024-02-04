<!--------------------------------
 - @Author: Ronnie Zhang
 - @LastEditor: Ronnie Zhang
 - @LastEditTime: 2023/12/05 21:28:02
 - @Email: zclzone@outlook.com
 - Copyright © 2023 Ronnie Zhang(大脸怪) | https://isme.top
 --------------------------------->

<template>
  <div>
    <n-upload multiple class="" :custom-request="handleUpload" :show-file-list="false" accept=".png,.jpg,.jpeg"
      @before-upload="onBeforeUpload">
      <n-upload-dragger>
        <div class="p-20">
          <!-- <i class="i-mdi:upload mb-12 text-68 color-primary" /> -->
          <n-text class="">点击或者拖动文件到该区域来上传</n-text>
        </div>
      </n-upload-dragger>
    </n-upload>

    <div class="mt-5">
    <draggable :list="imgList" animation="500" @start="drag = true" @end="drag = false" item-key="url" tag="div"
      class='flex'>
      <template #item="{ element, index }">
        <n-card class="cursor-move mr-1" style="width: 250px;">
          <div>
            <n-image width="200" :src="element.url" />
          </div>
          <n-space class="mt-6" justify="space-evenly">
            <n-button size="small" dashed type="primary" @click="deleteImg(index)">删除</n-button>
            <!-- <n-button dashed type="primary" @click="copy(item.url)">url</n-button>
            <n-button dashed type="primary" @click="copy(`![${item.fileName}](${item.url})`)">
              MD
            </n-button>
            <n-button dashed type="primary" @click="copy(`&lt;img src=&quot;${item.url}&quot; /&gt;`)">
              img
            </n-button> -->
          </n-space>
        </n-card>
      </template>
    </draggable>
  </div>
  </div>
</template>

<script setup>
import { useClipboard } from '@vueuse/core'
// defineOptions({ name: 'ImgUpload' })
import draggable from 'vuedraggable'
import { reactive, watch } from 'vue'
const { copy, copied } = useClipboard()

const imgList = reactive([
  { url: 'https://tse4-mm.cn.bing.net/th/id/OIP-C.2IYXnH0hH9RsOOIKgn5txQHaDQ?w=350&h=154&c=7&r=0&o=5&pid=1.7' },
  { url: 'https://tse4-mm.cn.bing.net/th/id/OIP-C.0AmDWSKuO8bbrw53cPe2awHaCo?w=305&h=124&c=7&r=0&o=5&pid=1.7' },
  { url: 'https://tse3-mm.cn.bing.net/th/id/OIP-C.ovPKzJOuIzsWkptVlapY4wHaH0?w=125&h=180&c=7&r=0&o=5&pid=1.7' },
])

watch(copied, (val) => {
  val && $message.success('已复制到剪切板')
})

function deleteImg(index) {
  imgList.splice(index, 1)
}

function onBeforeUpload({ file }) {
  if (!file.file?.type.startsWith('image/')) {
    $message.error('只能上传图片')
    return false
  }
  return true
}

async function handleUpload({ file, onFinish }) {
  if (!file || !file.type) {
    $message.error('请选择文件')
  }

  // 模拟上传
  $message.loading('上传中...')
  setTimeout(() => {
    $message.success('上传成功')
    imgList.push({ fileName: file.name, url: URL.createObjectURL(file.file) })
    onFinish()
  }, 1500)
}
</script>
