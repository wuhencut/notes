今天看到群里有人在问图片上传这个问题，自己一直有想了解，刚好群里有人给出了解决方法，我这里做一遍来加深印象

demo

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>UploadFile</title>
</head>
<body>
   <div>
        <img src="" id="img" style="width:200px;height:300px;" />
    </div>
    <input type="file" id="file" />
</body>
</html>
<script src="https://cdn.bootcss.com/jquery/1.12.1/jquery.js"></script>
<script type="text/javascript">
    window.onload = function () {
        var fileTag = document.getElementById('file');
        fileTag.onchange = function () {
            var file = fileTag.files[0];
            var fileReader = new FileReader();
            fileReader.onloadend = function () {
                if (fileReader.readyState == fileReader.DONE) {
                    console.log(fileReader);
                    console.log(file)
                    document.getElementById('img').setAttribute('src', fileReader.result);
                    $.post("http://localhost/index.php",{img:fileReader.result},function(result){
                        $("span").html(result);
                    });
                }
            };
            fileReader.readAsDataURL(file);
        };
    };
</script>
```

fileReader API文档： https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader

这里用的是fileReader，返回的fileReader.result是该文件的二进制流

发送给后台： post， 后台： base64_decode可以上传到服务器，不过好像我看群里有人说，微信小程序不可以用这个方法，之后再说

```javascript
header('Access-Control-Allow-Origin:*');
header('Access-Control-Allow-Methods:GET,POST,PATCH,PUT,OPTIONS');


$img = isset($_POST['img'])? $_POST['img'] : '';  

// 获取图片  
list($type, $data) = explode(',', $img);  

// 判断类型  
if(strstr($type,'image/jpeg')!=''){  
  $ext = '.jpg';  
}elseif(strstr($type,'image/gif')!=''){  
  $ext = '.gif';  
}elseif(strstr($type,'image/png')!=''){  
  $ext = '.png';  
}  

// 生成的文件名  
$photo = time().$ext;  

// 生成文件  
file_put_contents($photo, base64_decode($data), true);  


// 返回  
$ret = array('img'=>$photo,'code'=>1);  
echo json_encode($ret);
```
