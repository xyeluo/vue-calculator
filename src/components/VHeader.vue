<template>
  <header>
    <div class="title">
      <h1>计算器</h1>
      <div class="right">
        <ul>
          <li><span class="iconfont icon-jianhao"></span></li>
          <li><span class="iconfont icon-checkbox"></span></li>
          <li class="close"><span class="iconfont icon-cuocha_kuai"></span></li>
        </ul>
      </div>
    </div>
    <div class="func_zone">
      <div class="types"><span class="iconfont icon-daohang"></span></div>
      <div class="current_type">
        <h2>标准</h2>
      </div>
      <div class="top"></div>
      <div class="history"><span class="iconfont icon-history"></span></div>
    </div>
  </header>
</template>

<script>
</script>

<style scoped>
header {
  position: relative;
}
.title h1,
.right .iconfont {
  font-size: 13px;
}
.title h1 {
  font-weight: 400;
  padding: 10px;
}
.right {
  position: absolute;
  right: 0;
  top: 0;
}
.right li {
  display: inline-block;
  padding: 10px 18px;
}
.right .icon-checkbox {
  font-weight: 600;
  font-size: 12px;
}
.right li:hover {
  background-color: #a1a1a1;
}
.right .close:hover {
  background-color: #c42b1c;
}

.func_zone div {
  display: inline-block;
  padding: 6px 12px;
  line-height: 30px;
  border: 1.5px solid transparent;
}
.func_zone h2 {
  font-size: 24px;
}
.func_zone .top {
  --img_size: 20px;
}
.func_zone .top::after {
  content: '';
  display: inline-block;
  width: var(--img_size);
  height: var(--img_size);
  background: url('@/assets/top.jpg') no-repeat center 3px / var(--img_size) var(--img_size);
}
.func_zone .history {
  position: absolute;
  right: 0;
  padding: 6px 14px;
}
.func_zone .top:hover::after {
  background: url('@/assets/top_hover.jpg') no-repeat -1px 2.5px / var(--img_size) var(--img_size);
}
.func_zone div:hover {
  background-color: #acacac;
}
.func_zone div.current_type:hover{
  background-color:#b9b9b9;
}
.func_zone div.top:hover,.func_zone div.history:hover{
  border-color: #8b8989;
}
</style>