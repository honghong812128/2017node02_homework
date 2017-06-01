## 插件安装 (依靠formidable)
```
npm init -y
npm install mime formidable bootstrap -S
```

## index.html 注意事项
- table.table table-bordered
- area.addEventListener('dragover') area.addEventListener('drop')
- ajax && FormData && xhr.upload.onprogress
``` upload.addEventListener('click',function () {
           let xhr = new XMLHttpRequest();
           xhr.open('POST','/upload',true);
           //formData h5
           let fd = new FormData; //构建formData容器
           fd.append('file',file); //追加到容器中
           xhr.onload = function () {
               alert(xhr.response);
           };
           //只要进度改变就会触发此函数
           xhr.upload.onprogress = function (e) { // e.loaded  e.total
               let percent = e.loaded/e.total*100+"%";
               progress.style.width = percent;
               progress.innerHTML = (e.loaded/e.total*100).toFixed(2)+"%";
           };
           xhr.send(fd); //将内容送到服务端
       })

```

## server.js 注意事项
- fs.exists(pathname.slice(1))
- fs.createReadStream('.'+pathname).pipe(res);
- fs.createReadStream(files.file.path).pipe(fs.createWriteStream('./upload/'+files.file.name));
- let form = new formidable.IncomingForm();
```
let form = new formidable.IncomingForm();
form.parse(req,function (err,fileds,files) {
    fs.createReadStream(files.file.path).pipe(fs.createWriteStream('./upload/'+files.file.name));
});
form.on('end',function () {
    res.end('上传成功');
});
```