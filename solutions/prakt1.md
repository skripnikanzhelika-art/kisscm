## Задача 1
Вывести отсортированный в алфавитном порядке список имён пользователей в файле `/etc/passwd`.

#!/bin/bash
cut -d: -f1 /etc/passwd | sort


## Задача 2
 Вывести данные `/etc/protocols` для 5 наибольших портов:

#!/bin/bash
sort -k2 -n /etc/protocols | tail -n 5 | tac

## Задача 3
 Написать программу `banner` 
 
#!/bin/bash
text="$1"
len=${#text}
line=$(printf '%*s' "$((len + 2))" '' | tr ' ' '-')
echo "+${line}+"
echo "| ${text} |"
echo "+${line}+"

## Задача 4
 Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).


#!/bin/bash
file="$1"
if [ -z "$file" ]; then
    echo "Использование: $0 <файл>"
    exit 1
fi
grep -oE '\b[A-Za-z_][A-Za-z0-9_]*\b' "$file" | sort -u

## Задача 5
 Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в `/usr/local/bin`).

#!/bin/bash
if [ -z "$1" ]; then
    echo "Использование: $0 <файл>"
    exit 1
fi
src="$1"
name=$(basename "$src")
chmod +x "$src"
sudo cp "$src" /usr/local/bin/"$name"
echo "Команда $name установлена в /usr/local/bin"

## Задача 6
 Написать программу для проверки наличия комментария в первой строке файлов с расширением `c`, `js` и `py`.

#!/bin/bash
dir="${1:-.}"
find "$dir" -type f \( -name "*.c" -o -name "*.js" -o -name "*.py" \) | while read -r f; do
first=$(head -n 1 "$f")
case "$first" in
\#*|//*|/\**)
echo "$f: есть комментарий"
;;
*)
echo "$f: комментария нет"
;;
esac
done

## Задача 7
 Написать программу для нахождения файлов-дубликатов по заданному пути (и подкаталогам).


#!/bin/bash
dir="${1:-.}"
find "$dir" -type f -exec md5sum {} + | sort | uniq -w32 -D

## Задача 8
 Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента, и архивирует все эти файлы в архив tar.

#!/bin/bash
dir="$1"
ext="$2"
if [ -z "$dir" ] || [ -z "$ext" ]; then
    echo "Использование: $0 <каталог> <расширение>"
    exit 1
fi
ext="${ext#.}"
archive="archive_${ext}.tar"
find "$dir" -type f -name "*.${ext}" -print0 | tar --null -cvf "$archive" -T -
echo "Создан архив $archive"

## Задача 9
 Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции.

#!/bin/bash
if [ -z "$1" ] || [ -z "$2" ]; then
    echo "Использование: $0 <входной файл> <выходной файл>"
    exit 1
fi
sed 's/    /\t/g' "$1" > "$2"

## Задача 10
 Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории

#!/bin/bash
dir="$1"
if [ -z "$dir" ]; then
    echo "Использование: $0 <каталог>"
    exit 1
fi
find "$dir" -type f -empty -print
