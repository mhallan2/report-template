# Работа с буквами «ё» и «е»

Все команды необходимо выполнять из корня отчёта.

## Проверка наличия «ё»

Показать файлы и строки, содержащие строчную или заглавную букву «ё»:

```bash
grep -RInE --include='*.tex' --include='*.py' 'ё|Ё' .
```

## Замена без резервных копий
Заменить ё на е, а Ё на Е во всех файлах .tex и .py:

```bash
find . \
    -path './.git' -prune -o \
    -type f \( -name '*.tex' -o -name '*.py' \) \
    -exec sed -i 's/ё/е/g; s/Ё/Е/g' {} +
```

## Замена с резервными копиями
Вариант с созданием файлов .bak:

```bash
find . \
    -path './.git' -prune -o \
    -type f \( -name '*.tex' -o -name '*.py' \) \
    -exec sed -i.bak 's/ё/е/g; s/Ё/Е/g' {} +
```


После замены можно проверить git diff,

```bash
grep -RInE --include='*.tex' --include='*.py' 'ё|Ё' .
```


## Восстановление резеревных копий

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