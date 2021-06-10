大家应该都知道在script标签内有这两个属性async和defer，例如<script src="./home.js" async defer></script>

defer：中文意思是延迟。
    * 告诉浏览器理解加载，但延迟执行，
    当整个 document 解析完毕后再执行脚本文件，在 DOMContentLoaded 事件触发之前完成
    * 多个脚本按顺序执行

async：中文意思是异步

* async只适用于外部脚本文件，并告诉浏览器立即下载文件
* 不保证执行顺序