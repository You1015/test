<script setup>
import { ref } from 'vue'
import illustration from '@/assets/images/f3b98a45b95147719af263d1e992460c384c379e.png'

const fileList = ref([])

const beforeUpload = (file) => {

  const uid = Date.now() + Math.random()
  const url = URL.createObjectURL(file)
  const item = {
    name: file.name,
    size: file.size,
    uid,
    url,
    status: 'finished',
    raw: file,
  }
  fileList.value = fileList.value.concat(item)
  return false
}

const handleRemove = (file) => {

  const idx = fileList.value.findIndex((f) => f.uid === file.uid || f.name === file.name)
  if (idx !== -1) {
    try { URL.revokeObjectURL(fileList.value[idx].url) } catch (e) { /* ignore */ }
    fileList.value.splice(idx, 1)
  }
}

const handlePreview = (file) => {

  if (file && file.url) window.open(file.url, '_blank')
}
</script>

<template>
  <div class="page">
    <div class="left">
      <div class="top-blocks">
        <div class="block">
          <div class="block-title">Finish!</div>
          <div class="block-title">Upload Your Resume</div>
          <div class="block-sub">Upload your resume, the platform will help you parse and optimize,you can also skip this step</div>
        </div>

      </div>

      <div class="illus">
        <img :src="illustration" alt="illustration" />
      </div>
    </div>

    <div class="right">
      <div class="card">
        <h3 class="card-title">Upload file</h3>

        <el-upload
          class="upload-area"
          drag
          multiple
          :file-list="fileList"
          v-model:file-list="fileList"
          :before-upload="beforeUpload"
          :on-remove="handleRemove"
          :on-preview="handlePreview"
          list-type="picture"
        >
          <div class="upload-inner">
            <svg width="48" height="48" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 3v9" stroke="#2563EB" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M8 7l4-4 4 4" stroke="#2563EB" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" stroke="#60A5FA" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <p class="upload-text">Drag your resume file here, or click to select the file to upload</p>
          </div>
        </el-upload>

        <div class="actions">
          <el-button type="info" class="mr">Last step</el-button>
          <el-button type="primary">Finish</el-button>
        </div>

        <div class="progress">
          <div class="dot done"></div>
          <div class="line"></div>
          <div class="dot done"></div>
          <div class="line"></div>
          <div class="dot"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page{
  display:flex;
  gap:40px;
  align-items:stretch;
  min-height:100vh;
  padding:56px 80px;
  background:#F6FBFA;
  box-sizing:border-box;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}
.left{
  flex:1.1;
  display:flex;
  flex-direction:column;
  justify-content:flex-start;
  position:relative;
  padding-top:24px;
}
.top-blocks{display:flex; flex-direction:column; gap:6px; margin-bottom:28px}
.block .block-title{font-size:32px; font-weight:600; color:#0B1B2B; line-height:1}
.block .block-sub{font-size:22px; color:#0B1B2B; margin-top:6px}
.top-blocks .hint{color:#9AA8B6; margin-top:12px}
.illus{position:relative; margin-top:18px}
.illus img{width:620px; max-width:100%; display:block}

.right{flex:1.3; display:flex; align-items:flex-start; justify-content:center}
.card{width:100%; max-width:700px; background:white; border-radius:20px; padding:28px 36px; box-shadow:0 12px 40px rgba(19,49,71,0.07); display:flex; flex-direction:column}
.card-title{margin:0 0 20px; color:#111827; font-size:18px; font-weight:700}
.upload-area{border:2px dashed rgba(47,139,255,0.25); border-radius:12px; padding:48px; display:flex; align-items:center; justify-content:center; min-height:340px; background:linear-gradient(180deg,#F8FBFF, #F5FAFF); flex:1}
.upload-inner{text-align:center; color:#93A0AD; max-width:420px}
.upload-inner svg{display:block; margin:0 auto 10px}
.upload-text{font-size:13px; color:#97A6B3; line-height:1.6}
.actions{display:flex; gap:16px; margin-top:28px; justify-content:center}
.actions .el-button{border-radius:8px; padding:10px 28px; font-weight:600; transition:transform .14s cubic-bezier(.2,.9,.2,1), box-shadow .14s ease, filter .12s ease, opacity .12s ease}
.actions .el-button--info{background:linear-gradient(180deg,#BBD7FF,#8FB7FF); color:#0F1724; border:none}
.actions .el-button--primary{background:linear-gradient(180deg,#2F80ED,#1C64E4); color:#fff; border:none; box-shadow:0 10px 24px rgba(46,83,212,0.18)}
.actions .el-button:focus{box-shadow:none}


.actions .el-button:hover{transform:translateY(-4px)}
.actions .el-button:active{transform:translateY(-1px); opacity:0.98}
.actions .el-button--info:hover{box-shadow:0 10px 28px rgba(47,139,255,0.12); filter:brightness(0.98)}
.actions .el-button--primary:hover{box-shadow:0 16px 36px rgba(46,83,212,0.22); filter:brightness(0.98)}
.actions .el-button--info:active{box-shadow:0 6px 18px rgba(47,139,255,0.08)}
.actions .el-button--primary:active{box-shadow:0 8px 22px rgba(46,83,212,0.12)}


.actions .el-button:focus-visible{outline:3px solid rgba(47,139,255,0.14); outline-offset:4px}

.progress{display:flex; align-items:center; gap:12px; justify-content:center; margin-top:34px}
.dot{width:18px; height:18px; border-radius:50%; background:#fff; border:3px solid #E6EEF9; box-shadow:0 4px 10px rgba(12,36,60,0.06)}
.dot.done{border-color:#2F80ED}
.line{height:3px; background:#E6EEF9; flex:1; max-width:110px; border-radius:2px}

@media (max-width:1100px){
  .page{flex-direction:column; padding:24px}
  .illus img{max-width:520px}
  .right{margin-top:24px}
  .card{max-width:100%}
}

</style>
