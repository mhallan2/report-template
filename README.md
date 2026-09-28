# Шаблон отчёта

## Консоль Bash через minted

Шаблон содержит окружение `consolebox`: оно подсвечивает приглашение Linux
(`user@host:~/path$`) и команду как Bash, а остальные строки считает выводом
команды. Для него нужен локальный пакет Pygments из `terminal_theme`.

Один раз установите его из корня отчёта:

```bash
python -m pip install ./terminal_theme
```

Для изменения стиля во время разработки используйте переустановку:

```bash
python -m pip install --upgrade --force-reinstall ./terminal_theme
```

Проверить, что Pygments видит компоненты, можно командами:

```bash
pygmentize -L lexers | grep -i linuxconsole
pygmentize -L styles | grep -i linuxterm
```

Пример в документе:

```tex
\begin{consolebox}
user@host:~/project$ python script.py
Готово
\end{consolebox}
```

Сборка уже настроена в `.latexmkrc` с `-shell-escape`, который необходим
`minted`.

Все команды необходимо выполнять из корня отчёта.

## Работа с буквами «ё» и «е»

### Проверка наличия «ё»

Показать файлы и строки, содержащие строчную или заглавную букву «ё»:

```bash
grep -RInE --include='*.tex' --include='*.py' 'ё|Ё' .
```

### Замена без резервных копий
Заменить ё на е, а Ё на Е во всех файлах .tex и .py:

```bash
find . \
    -path './.git' -prune -o \
    -type f \( -name '*.tex' -o -name '*.py' \) \
    -exec sed -i 's/ё/е/g; s/Ё/Е/g' {} +
```

### Замена с резервными копиями
Вариант с созданием файлов .bak:

```bash
find . \
    -path './.git' -prune -o \
    -type f \( -name '*.tex' -o -name '*.py' \) \
    -exec sed -i.bak 's/ё/е/g; s/Ё/Е/g' {} +
```


После замены можно проверить 
```bash
git diff
```
или

```bash
grep -RInE --include='*.tex' --include='*.py' 'ё|Ё' .
```


### Восстановление резеревных копий

```bash
find . \
    -type f -name '*.bak' \
    -exec sh -c '
        for backup do
            mv -f -- "$backup" "${backup%.bak}"
        done
    ' sh {} +
```

Удаление:
```bash
find . -type f -name '*.bak' -delete
```
```
